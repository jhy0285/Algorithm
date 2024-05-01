#  📚 Stack 


## 특징

### 후입선출(LIFO Last In First Out) 구조

`Stack`(이하 '스택')이란 데이터를 쌓아올린 형태의 자료구조를 뜻한다.  
데이터를 한 방향으로만 저장 할 수 있고 `최상층(Top)`으로 정한 곳에 위치한 데이터만 `삽입/조회/삭제` 할 수 있다.

## 주요 메서드(Java)

1. `isEmpty()`, `isFull()`
   - 각각 스택이 비어있는지, 가득찼는지를 boolean 형태로 리턴하는 메서드.
2. `push()`
   - 스택에 새로운 원소를 삽입한다. 
   - 가득 차 있다면 예외를 던진다.
3. `peek()`
   - 최상층에 위치한 데이터를 읽어온다.
4. `pop()`
   - 최상층에 위치한 데이터를 읽어오고 해당 데이터를 스택에서 제거한다.

## Java 의 Stack 구현

### 인터페이스 정의

```
public interface MyStack<T> {
    boolean isEmpty();
    boolean isFull();
    void push(T element);
    T peek();
    T pop();
    void clear();
}
```

### 클래스 구현

```
public class MyStackImpl<T> implements MyStack<T> {

    private List<Optional<T>> myStack;
    private int limit;

    public MyStackImpl(int size) {
        this.myStack = new LinkedList<>();
        this.limit = size;
    }

    @Override
    public boolean isEmpty() {
        return this.myStack.isEmpty();
    }

    @Override
    public boolean isFull() {
        return this.myStack.size() == limit;
    }

    @Override
    public void push(T element) throws FullException {
        if (this.myStack.size() == limit) {
            throw new FullException();
        }
        this.myStack.add(Optional.ofNullable(element));
    }

    @Override
    public T peek() throws EmptyException {
        try {
            return this.myStack.get(myStack.size() - 1).orElseThrow(EmptyException::new);
        } catch (IndexOutOfBoundsException e) {
            throw new EmptyException();
        }
    }

    @Override
    public T pop() throws EmptyException {
        try {
            return myStack.remove(myStack.size() - 1).orElseThrow(EmptyException::new);
        } catch (IndexOutOfBoundsException e) {
            throw new EmptyException();
        }
    }

    @Override
    public void clear() {
        myStack.clear();
    }
}
```

### 예외 처리

```
public class FullException extends RuntimeException {

    public FullException() {}

    public FullException(String message) {
        super(message);
    }
}
```

```
public class EmptyException extends RuntimeException {

    public EmptyException() {}

    public EmptyException(String message) {
        super(message);
    }
}
```


## 예상 질문

### Q1. 스택으로 큐를 구현 할 수 있는가? 그 반대는?

> 스택 두개를 사용 해 구현 가능합니다.

### Q2. 스택과 큐의 특성을 모두 사용해야한다면?

> `데크(Deque)`자료구조를 사용 할 수 있습니다. 삽입, 삭제, 조회 연산이 데크의 양 끝단에서만 이루어지는 구조입니다.