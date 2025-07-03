

2025-04-18

----
#멀티스레딩 #아키텍처

## 개요
잠겨있는 객체에 대해 동일 스레드에서 재진입을 시도하면 어떤 오류가 뜨는지 확인해 보자. 

## 내용
다음은 재진입성의 예시.
```cpp
#include <mutex>

using namespace std;

class ReenterancyTest
{
public:
	void foo1()
	{
		scoped_lock<mutex> lock(mtx);
		
		foo2();
		m_test++;
		cout << m_test << endl;
	}
	void foo2()
	{
		scoped_lock<mutex> lock(mtx);
		
		m_test++;
		cout << m_test << endl;
	}

private:
	mutex mtx;
	int m_test{ 0 };
};

int main()
{
	auto test = new ReenterancyTest();
	test->foo1();
}
```

실행 화살표가 `foo2`의 `scoped_lock<mutex> lock(mtx);`에 멈춰 있을 때까진 괜찮지만 F10을 눌러 해당 행을 실행하면 `std::system_error`가 뜨면서 터진다. 