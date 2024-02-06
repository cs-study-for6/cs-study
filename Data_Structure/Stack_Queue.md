# :books: Stack
## 정의
한쪽 끝에서만 자료를 넣거나 뺄 수 있는 자료구조
## 특징
- 후입선출 (LIFO, Last-In-First-Out) 구조
- push 연산으로 삽입, pop 연산으로 추출
- 비어있는 스택에서 원소를 추출하려고 하면, `stack underflow`가 발생한다.
- 스택이 꽉 차 있을 때 원소를 삽입하려고 하면, `stack overflow`가 발생한다.
- 시간 복잡도: O(1)
## 활용 예시
- 웹 브라우저의 뒤로 가기 기능
- 실행 취소 기능
- 재귀 알고리즘 / DFS
- 후위 표기법 계산
- 괄호 검사
## 자바에서의 구현
1. Stack 클래스를 사용
```java
import java.util.Stack;

class StackTest{
    public static void main(String args[]){
        // stack 선언
        Stack<Integer> stack = new Stack<>();
        // 값 삽입
        stack.push(1);
        stack.push(2);
        // 추출 -> 반환값 2
        stack.pop();
    }
}
```
2. ArrayDeque 클래스를 사용
```java
import java.util.ArrayDeque;

class DequeTest{
    public static void main(String args[]){
        // stack 선언
        Stack<Integer> stack = new ArrayDeque<Integer>();
        // 값 삽입
        stack.push(1);
        stack.push(2);
        // 추출 -> 반환값 2
        stack.pop();
    }
}
```

# :monorail: Queue
## 정의
한쪽 끝(rear)에서는 삽입만 이루어지고, 다른 한쪽 끝(front)에서는 삭제만 이루어지는 자료 구조
## 특징
- 선입선출 (FIFO, First-In-First-Out) 구조
- enQueue 연산으로 삽입, deQueue 연산으로 추출
- 시간 복잡도: O(1)
## 활용 예시
- 명령 순차 처리
- 캐시 구현
- BFS
## 자바에서의 구현
1. LinkedList 클래스를 사용
```java
import java.util.Queue;
import java.util.LinkedList;

class QueueTest{
    public static void main(String args[]){
        // queue 선언
        Queue<Integer> queue = new LinkedList<>();
        // 값 삽입
        // offer / add 사용
        queue.offer(1);
        queue.add(2);
        // 추출 -> 반환값 1
        queue.poll();
    }
}
```
2. ArrayDeque 클래스를 사용
```java
import java.util.Deque;
import java.util.ArrayDeque;

class DequeTest{
    public static void main(String args[]){
        // queue 선언
        Queue<Integer> queue = new ArrayDeque<Integer>();
        // 값 삽입
        // offer / add 사용
        queue.offer(1);
        queue.add(2);
        // 추출 -> 반환값 1
        queue.poll();
    }
}
```


