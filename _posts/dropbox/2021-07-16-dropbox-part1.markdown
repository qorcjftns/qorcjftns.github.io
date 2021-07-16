---
layout: post
type:   posts
title:  "[드랍박스 인터뷰 준비] ID Allocator"
date:   "2021-07-16 10:16:11"
categories:
  - "드랍박스"
tags:
  - 잡담
  - 아이디어
  - 드롭박스
  - 인터뷰
  - 면접
  - 알고리즘
---

## 서론
[드랍박스 인터뷰 문제](https://github.com/insideofdrop/Dropbox-Interview-Prep) 모음집의 가장 첫번째 문제가 바로 이 ID Allocator 문제이다. 다양한 인터뷰 후기를 읽어보아도 왠만하면 나오는 문제로 보여졌다. 그리고 다행(?)히도, 이 문제는 실제로 내가 다양한 실전 상황에서 구현을 해본 문제이다. 이 문제는 단순해보이지만 다양한 해법이 있기 때문에 푸는 사람의 성향을 파악하기가 쉬운 문제라고 생각한다.

ID를 allocate만 한다면 이 문제에 어려운 점은 없겠지만, 이런 경우에는 항상 deallocate되는 경우까지 고려해서 문제가 발생한다. 특정 ID가 deallocate되었을때 그 ID가 어떻게 처리되어야 할지, 그리고 그 처리 방식을 얼마나 효율적으로 처리할지가 관건이 될 것이다.

### Github의 문제 풀이

해당 문서에서 제안한 풀이방식은 총 세가지였다.

1. queue와 set를 이용한 빈 ID 처리
2. Boolean array를 이용한 ID 처리
3. Binary Heap을 이용한 ID 처리

다양한 방식의 해결법이 주어졌지만, 개인적으로는 모두 만족하지 못했다.

1번 방법은 우선 alloc은 시간복잡도 O(1) 이내로 해결되었지만 dealloc이 O(n)이 걸린다. Set에서 하나의 element를 삭제하는것이 O(n)이 걸리기 때문이다. 공간 또한 queue와 set을 둘다 사용하기 때문에 그렇게 효율적인 편이 아니라고 생각된다.

2번 방법은 공간적으로는 매우 우수하다. MAXID만큼의 bit만 있으면 되기 때문에 공간적으로 이 방법을 따라올수는 없다. 다만, alloc이 매우 비효율적인데, alloc할때마다 어레이를 full iterate해야하기 때문에 O(n)의 시간복잡도를 가지게 된다. 반면 dealloc은 O(1)이기 때문에 1번과 동일하다. 종합적으로는 1번 방법보다 공간을 덜 차지하기 때문에 전반적으로는 1번보다 효율적이라고 볼 수 있다.

3번 방법은 Binary Heap을 사용하는데, 속도는 O(n)보다는 빠른 O(logn)이 나오게 된다. 2번과 비슷하게 bit array를 사용하지만 공간을 두배로 할당해준다. 이것은 물론 O(n)과 O(1)을 사용하는 위 두가지보다는 낫지만, 공간복잡도가 두배로 상승하는 문제가 생겨버린다.



### 나의 문제 풀이
위 세가지 다 장단점이 명확한 방식들이고, 상황에 따라서 어떤것을 쓰게 될지는 다 다른 방식이라고 생각한다. 하지만 아무래도 모두 다 프로그램 규모가 커질수록 문제가 생기게 된다.

기존에 주어진 방식에서 조금 더 개선해서 내가 떠올린 방법은 아래와 같다:

{% highlight cpp %}
class IDAllocator {
private:
	int MAXID, cur_new;
	queue<int> empty;
	bool *ids;
	
public:
	IDAllocator(int max_id) {
		MAXID = max_id;
		cur_new = 0;
		ids = new bool[max_id];
	}
	~IDAllocator() {
		delete ids[];
	}
	
	int alloc() {
		int result_id;
		if(!empty.empty()) { 
			// when queue is not empty
			result_id = empty.front();
			empty.pop();
		} else if(cur_new <= MAXID) {
			// when queue is empty and ID is available
			cur_new += 1;
			result_id = cur_new;
		} else {
			// when here is no ID available at all
			throw "ID Allocation Failed";
		}
		ids[result_id] = true;
		return result_id;
	}
	
	void dealloc(int id) {
		if(id > cur_new) throw "ID Deallocation Failed";
		else if(!ids[id]) throw "ID Deallocation Failed";
		else empty.push(id);
	}
}
{% endhighlight %}

Github repo에 있는 1번 방식과 거의 흡사하지만, 가장 큰 차이점은 allocated ID를 트랙하지 않는다. 우선 이 class의 member는 queue 하나와 cur_new와 MAXID, 그리고 ids라는 bool array이다.

ids 어레이는 말그대로 특정 id가 사용중인지 아닌지만 판단하는 bit 어레이이다. 이 변수는 cpp의 특성상 new로 생성하였기 때문에 모두 false(0)로 초기화되어 있게 될 것이다.

empty queue는 deallocate된 ID들을 보관해두는 queue이다. 당연하게도 처음에는 아무것도 들어있지 않으며, dealloc이 콜될때마다 ID가 push된다.

cur_new는 가장 최근에 생성한 ID를 보관하는 커서 변수이다. 한번 MAXID까지 ID가 생성된 경우에는 더이상 사용되지 않는다.

### alloc
alloc 메서드는 단순한 원리로 작동된다. 우선 empty queue에 ID가 존재하는 경우, queue에서 ID를 pop해서 리턴해주면 된다. 혹은 empty queue가 사이즈가 0인 경우, cur_new라는 커서를



