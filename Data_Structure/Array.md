# CS

## 배열(Array)

- 선형 자료구조
    
    ![Linear.png](CS%20b8bfc258fc4a47508d08125b1eb04ad8/Linear.png)
    
    - 선형 자료 구조는 자료를 구성하는 데이터가 일렬로 나열되어 있는 자료 구조로
    하나의 자료 뒤에 하나의 자료가 존재한다.
    - 자료들 간의 앞뒤 관계가 1 : 1의 선형관계이다.
    - 배열과 리스트가 대표적이고 더 나아가 스택, 큐도 이에 해당된다.
- 가장 기본적인 자료구조
- 논리적 저장 순서와 물리적 저장 순서가 일치한다.
- 크기가 정해져 있다.
- 중복을 허용하며 순서가 있다.
- 삽입/삭제 : O(n)
- 검색 : O(1)
    
    index를 이용해 접근하기에 O(1)
    

- 메소드 구현하기
    - add(int num)
        
        ```java
        public void addElement(int num) {
            //배열의 요소를 추가하는 메소드
            if(count >= ARRAY_SIZE){
                //만약 요소의 개수가 지정된 배열의 크기보다 크거나 같으면 에러 출력
                System.out.println("not enough memory");
                return;
            }
            //현재 요소 개수에 해당하는 자리에 넣고 count 1 증가
            intArr[count++] = num;
        }
        ```
        
    
    - insert(int position, int num)
        
        ```java
        public void insertElement(int position, int num) {
            // 특정 위치에 요소를 추가하는 메소드
            int i;
        
        		// 꽉 찬 경우
            if(count >= ARRAY_SIZE){  
                System.out.println("not enough memory");
                return;
            }
        
        		// index error
            if(position < 0 || position > count ){  
                System.out.println("insert Error");
                return;
            }
        
            for( i = count-1; i >= position ; i--){
                // 배열의 제일 끝 부분부터 삽입할 위치(position)까지 1씩 감소하면서
        				// 하나씩 이동 
        				// [i]에 있는 요소를 [i+1] 에 이동
                intArr[i+1]  = intArr[i]; 
        
            }
        
            // 넣고자 하는 위치(position)에 요소 값 넣기
            intArr[position] = num;
            count++; //요소의 수 1 증가
        }
        ```
        
    
    - remove()
        
        ```java
        public int removeElement(int position) {
            // 특정 위치의 요소를 제거하는 메소드
            int ret = ERROR_NUM;
        
            // 배열이 비어있다면
            if( isEmpty() ){
            System.out.println("There is no element");
                return ret;
            }
        
            // Index Error 에러 처리 
            if(position < 0 || position >= count ){  //index error
                System.out.println("remove Error");
                return ret;
            }
        
            // 제거할 위치의 요소를 ret 변수에 저장
            ret = intArr[position];
        
            for(int i = position; i<count -1; i++ ) {
                // 제거할 위치부터 배열의 제일 마지막 위치까지 1씩 증가
        				// 하나씩 이동 
        				// [i+1]에 있는 요소를 [i]에 이동
                intArr[i] = intArr[i+1]; 
            }
            count--; //요소의 수 1 감소
            return ret; //삭제된 요소 값 반환
        }
        ```
        
    
    - getSize()
        
        ```java
        public int getSize() {
            // 현재 요소 수 반환
            return count;
        }
        ```
        
    
    - isEmpty()
        
        ```java
        public boolean isEmpty() {
            // 비어있는지 체크
            if(count == 0){
                return true;
            }
            else return false;
        }
        ```
        
    
    - getElement()
        
        ```java
        public int getElement(int position) {
            // 특정 위치 값으로 요소 수 조회
            if(position < 0 || position > count-1){
                System.out.println("검색 위치 오류. 현재 리스트의 개수는 " + count +"개 입니다.");
                return ERROR_NUM;
            }
            return intArr[position];
        }
        ```
        

## 배열 리스트(ArrayList)

- List인터페이스를 구현하기 때문에 데이터의 저장 순서가 유지되고 중복을 허용한다.
- Object배열을 이용해서 데이터를 순차적으로 저장한다.
- Size가 고정되어 있어 삽입 시 사이즈를 늘려주는 연산이 추가되어야 한다.
- 삭제 시 순차적으로 저장되어있는 데이터 구조로 인해 삭제된 부분을 채워야하므로 연산이 또 추가된다.
- 따라서 ArrayList에서는 데이터의 삽입/삭제 효율이 낮아 잘 사용하지 않는다.

- 메소드 구현하기
    - add(E value)
        
        ```java
        @Override
        public boolean add(Object value) {
        		// 현재 배열이 꽉 차있는 상태이면 리사이징
            resize(); 
        
        		// size가 원소의 갯수이고, 배열의 인덱스는 0부터 시작하니 
        		// 결국 추가할 수 있는 마지막 위치를 가리키게 된다.
            elementData[size] = value;
        		// 원소가 추가되었으니, 배열 크기를 나타내는 size도 올린다.
            size++; 
            return true;
        }
        ```
        
    
    - add(int index, E value)
        - 중간에 데이터를 삽입하는 것은 확인해야하는 절차가 있다. 리스트는 **데이터가 연속하게 저장**되어 있는 배열인데 중간에 빈 공간이 있는 채로 요소가 들어있거나 음수가 들어오면 안되기 때문이다.
            1. 매개변수로 받아온 인덱스 범위가 음수와 같은 옳지 않은 값이 올 경우 
            (index < 0)
            2. 매개변수로 받아온 인덱스 범위가 현재 배열 용량(capacity)보다 초과되거나, 중간에 빈 공간이 남은 채로 끝 부분에 추가하는 경우 (index > size)
        
        ```java
        @Override
        public void add(int index, Object value) {
            // 인덱스가 음수이거나, 배열 크기(size)를 벗어난 경우 예외 발생 
        		// 리스트는 데이터가 연속되어야함
            if (index < 0 || index > size) {
                throw new IndexOutOfBoundsException();
            }
        
            // 인덱스가 마지막 위치일 경우
            if (index == size) {
                add(value); // 그냥 추가
            }
            // 인덱스가 중간 위치를 가리킬 경우
            else {
                resize(); // 현재 배열이 꽉 차있는 상태이면 리사이징
        
                // 루프변수에 배열 크기를 넣고, index 위치 까지 순회해서 요소들을
        				// 한 칸 씩 뒤로 밀어 빈 공간 만들기
                for (int i = size; i > index; i--) {
                    elementData[i] = elementData[i - 1];
                }
        
                elementData[index] = value; // index 위치에 요소 할당
                size++;
            }
        }
        ```
        
    
    - indexOf(Object value) - 순차검색
        - ArrayList는 유한한 값과 더불어 null도 저장할 수 있는 자료구조이다. 그런데 null 비교 같은 경우 동등 연산자로 사용해야하기 때문에 어쩔 수 없이 매개변수가 null일 경우와 실질적인 값인 경우를 나누어야 한다.
        
        ```java
        @Override
        public int indexOf(Object value) {
            // 매개변수가 null 일경우 
        		// null 비교는 동등연산자로 행하기 때문에 비교 로직을 분리
            if (value == null) {
                for (int i = 0; i < size; i++) {
                    if (elementData[i] == null) {
                        return i; // 인덱스 반환
                    }
                }
            }
            // 매개변수가 실질적인 값일 경우
            else {
                for (int i = 0; i < size; i++) {
                    if (elementData[i].equals(value)) {
                        return i; // 인덱스 반환
                    }
                }
            }
        
            return -1; // 찾은 값이 없을 경우
        }
        ```
        
    
    - LastindexOf(Object value) - 역순 검색
        - size -1에서부터 0까지 순회하며 역순으로 검색
        
        ```java
        @Override
        public int lastIndexOf(Object value) {
            if (value == null) {
                for (int i = size - 1; i >= 0; i--) {
                    if (elementData[i] == null) {
                        return i; // 인덱스 반환
                    }
                }
            } else {
                for (int i = size - 1; i >= 0; i--) {
                    if (elementData[i].equals(value)) {
                        return i; // 인덱스 반환
                    }
                }
            }
        
            return -1; // 찾은 값이 없을 경우
        }
        ```
        
    
    - remove(int index)
        
        ```java
        @Override
        @SuppressWarnings("unchecked")
        public E remove(int index) {
            // 1. 인덱스가 음수이거나, size 보다 같거나 클경우 
        		// size와 같다는 말은 요소 위치가 빈공간 이라는 말
            if (index < 0 || index >= size) {
                throw new IndexOutOfBoundsException();
            }
        
            // 2. 반환할 값 백업
            E element = (E) elementData[index];
        
            // 3. 요소 제거 
        		// 명시적으로 요소를 null로 처리해주어야 GC가 수거해감
            elementData[index] = null;
        
            // 4. 배열 요소 이동 
        		// 삭제한 요소의 뒤에 있는 모든 요소들을 한 칸씩 당겨옴
            for (int i = index; i < size - 1; i++) {
                elementData[i] = elementData[i + 1];
                elementData[i + 1] = null;
            }
        
            // 5. 요소를 제거했으니 size도 감소
            size--;
        
            // 6. 현재 배열이 capacity가 너무 남아도는 상태이면 리사이징
            resize();
        
            // 7. 백업한 삭제된 요소를 반환
            return element;
        }
        ```
        
    
    - remove(Object value)
        
        ```java
        @Override
        public boolean remove(Object value) {
            // 1. 먼저 해당 요소가 몇번째 위치에 존재하는지 인덱스를 얻어온다.
            int idx = indexOf(value);
        
            // 2. 만약 값이 -1이면 삭제하고자 하는 값이 없는 것이니 
        		// 그대로 메서드를 종료한다.
            if(idx == -1) return false;
        
            // 3. 인덱스를 찾았으면 그대로 remove() 메서드로 넘겨 삭제한다.
        		// remove() 에서 요소 삭제 및 size감소 리사이징 처리
            remove(idx);
        
            return true;
        }
        ```
        

## 연결 리스트(Linked List)

- 배열과 마찬가지로 선형 자료구조를 갖는다.
    
    ![linked list.webp](CS%20b8bfc258fc4a47508d08125b1eb04ad8/linked_list.webp)
    

- 데이터를 감싼 노드를 포인터로 연결해서 공간적인 효율성을 극대화시킨 자료구조이다.
- 논리적 저장 순서와 물리적 저장 순서가 일치하지 않는 자료구조이다.
- 각 원소의 다음 원소가 무엇인지만을 알고 있어 삽입/삭제 시 O(1)의 속도를 보여준다.
- 연속적이지 않은 특성 때문에 특정 원소에 접근할 때 해당 원소를 찾기 위해 모든 원소를
탐색해야해서 O(n)의 속도를 보여준다.
- 검색/삽입/삭제 모두 O(n)의 시간이 소요된다.
- Tree의 근간이 되는 자료구조로 유용하게 쓰인다.