#### One-hot encoding

one-hot encoding을 이용한 위치 엔코딩 방식은 전체의 순서 위치에 대해서 유일한 하나의 위치 값을 줄 수는 있지만, 입력값의 상호 순서간의 상관성을 가지게 하는데는 한계가 있다. 

그렇기 때문에 NLP에서는 적합하지 않으며, positional encoding을 통해서 절대적인 위치의 정보를 이용하는 방식보다는 상호간의 상대적인 위치 정보를 제공하는 기법을 활용한다. 



#### Positional encoding

* `$sin$` 함수는 continuous function이기 때문에 주어진 `$x$`(위치정보)에 따라 상호간의 관계를 부드럽게 연결할 수 있다.

* sinusoidal positional encoding: 하나의 `$sin$` 함수를 사용하면 서로 다른 위치에서 동일한 `$sin(x)$` 값을 가질 수가 있지만, 서로 다른 주기의 `$sin$` 함수를 조합하면 각 위치에서 모두 다른 유일한 값을 가지게할 수 있다. 

* `$sin$`과 `$cos$`을 모두 사용하는 이유: 주기함수는 위치가 커지면 값이 다시 작아졌다 커지는 반복을 하기 때문에 어떤 특정 두 토큰의 위치값이 동일해질 수 있는 것을 방지하기 위함이다. 따라서, sin과 cos을 함께 사용하여 일정하게 증가 또는 감소하게 표현할 수 있다. (즉, sin과 cos을 함께 사용해야 선형변환으로 표현이 가능하다는 것과 동일하다.)

* Concatenation 을 사용하지 않고 Adding을 사용하는 이유: embedded word의 차원이 큰 경우에는 Concatenation을 위해서는 PE (position encoding) 도 같은 크기의 메모리를 필요로하게 되어서, 큰 메모리 용량을 필요로 하게 되며 입력값의 차원이 커지게 되면, Self-Attention 계산에서 급격히 파리머터 갯수가 증가하게 된다. Concatenation 대신에 단순히 수치를 Adding을 하게 되면, positional encoding 정보를 더했지만, 원래 입력값의 차원을 유지하게 되어서, 내부적으로 처리될 Self-Attention 의 계산을 크게 간소화시킬 수 있다. 

* Multiply 를 사용하지 않고 Adding을 사용하는 이유: WE (word embedding)과 PE (position encoding)을 곱셈하지 않고 덧셈을 하는 이유는, 곱셈을 하면 PE의 수열에서 0인 부분이 있다면, 중요한 정보인 WE의 정보를 없애버릴 수 있기 때문으로, 곱셈보다 덧셈을 하면 중요한 정보인 WE의 정보를 유지하면서 위치 정보를 덧씌울 수 있다. 