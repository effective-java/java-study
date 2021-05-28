# item2

# 생성자에 매개변수가 많다면 빌더를 고려하라

### 정적 팩터리와 생성자의 제약

- 선택적 매개변수가 많을 때 적절히 대응하기 어려움
- ex) 영양정보를 표현하는 클래스
    - 필수 항목: 1회 내용량, n회 제공량, 1회 제공량당 칼로리
    - 선택 항목: 총 지방, 트랜스지방, 콜레스테롤, 나트륨 등 20가지 이상
    - 대다수 제품은 선택 항목 중 대다수의 값이 0
- 이런 클래스용 생성자 혹은 정적 팩터리는 주로 1. 점층적 생성자 패턴을 사용해왔음
    - 점층적 생성자 패턴(telescoping constructor pattern): 필수매개변수 생성자, 필수 + 선택 1 생성자... 형태로 선택 매개변수를 전부 다 받는 생성자까지 늘려가는 방식
    - 해당 클래스의 인스턴스를 만들려면 원하는 매개변수를 모두 포함한 생성자 중 가장 짧은 것을 골라 호출
    - 단점
        - 매개변수가 많아지면 클라이언트 코드를 작성하거나 읽기 어렵다

    ```java
    public class NutritionFacts{
        private final int servingSize;  // 필수
        private final int servings;     // 필수
        private final int calories;     // 선택
        private final int fat;          // 선택
        private final int sodium;       // 선택
        private final int carbohydrate; // 선택

        public NutritionFacts(int servingSize, int servings){
            this(servingSize, servings, 0);
        }

        public NutritionFacts(int servingSize, int servings, int calories){
            this(servingSize, servings, calories, 0);
        }

        public NutritionFacts(int servingSize, int servings, int calories,
                              int fat){
            this(servingSize, servings, calories, fat, 0);
        }

        public NutritionFacts(int servingSize, int servings, int calories,
                              int fat, int sodium){
            this(servingSize, servings, calories, fat, sodium, 0);
        }

        public NutritionFacts(int servingSize, int servings, int calories,
                              int fat, int sodium, int carbohydrate){
            this.servingSize = servingSize;
            this.servings = servings;
            this.calories = calories;
            this.fat = fat;
            this.sodium = sodium;
            this.carbohydrate = carbohydrate;
        }
    }
    ```

- 2. 자바 빈즈 패턴(javaBeans pattern)
    - 단점
        - 객체 하나를 만들려면 메서드를 여러 개 호출해야 하고, 객체가 완전히 생성되기 전까지는 일관성이 무너진 상태에 놓이게 된다
        - 이런 일관성이 무너지는 문제 때문에 클래스를 불변으로 만들 수 없으며 스레드 안정성을 얻으려면 추가 작업이 필요하다.
        - 생성이 끝난 객체를 수동으로 freezing하고 얼리기 전에는 사용할 수 없도록 하기도 하지만 freeze 메서드를 확실히 호출해쭷는지를 컴파일러가 보증 x, 런타임 오류 취약

    ```java
    class NutritionFacts{
        
        private int servingSize  = -1;  // 필수
        private int servings     = -1;  // 필수
        private int calories     = 0;   // 선택
        private int fat          = 0;   // 선택
        private int sodium       = 0;   // 선택
        private int carbohydrate = 0;   // 선택

        public NutritionFacts() { }

        public void setServingSize(int val) { servingSize = val; }

        public void setServings(int servings) { servings = val; }

        public void setCalories(int calories) { calories = val; }

        public void setFat(int fat) { fat = val; }

        public void setSodium(int sodium) { sodium = val; }

        public void setCarbohydrate(int carbohydrate) { carbohydrate = val; }
    ```

    ```java
    NutritionFacts cocaCola = new NutritionFacts();
    cocaCola.setServingSize(240);
    cocaCola.setServings(8);
    cocaCola.setCalories(100);
    cocaCola.setSodium(35);
    cocaCola.setCarbohydrate(27);
    ```

### 빌더 패턴(Builder pattern)

- 파이썬과 스칼라에 있는 명명된 선택적 매개변수(named optional parameters)를 흉내낸 것
    - *args, **kwargs
- 점층적 생성자 패턴의 안정성과 자바 빈즈 패턴의 가독성을 겸비한 패턴
    1. 클라이언트는 필요한 객체를 직접 만드는 대신, 필수 매개변수만으로 생성자(정적 팩토리)를 호출해 빌더 객체를 얻는다
    2. 빌더 객체가 제공하는 일종의 세터 메서드들로 원하는 선택 매개변수들을 설정한다
    3. 매개변수가 없는 build 메서드를 호출해 필요한(보통은 불변인) 객체를 얻는다.

      cf) 빌더는 생성할 클래스안에 정적 멤버 클래스로 만들어두는게 보통

    ```java
    class NutritionFacts{
        private final int servingSize;
        private final int servings;
        private final int calories;
        private final int fat;
        private final int sodium;
        private final int carbohydrate;

        public static class Builder{
            // 필수 매개변수
            private final int servingSize;
            private final int servings;

            // 선택 매개변수 - 기본값으로 초기화한다.
            private int calories     = 0;
            private int fat          = 0;
            private int sodium       = 0;
            private int carbohydrate = 0;

            public Builder(int servingSize, int servings) {
                this.servingSize = servingSize;
                this.servings = servings;
            }

            public Builder calories(int val){
                this.calories = val;
                return this;
            }

            public Builder fat(int val){
                this.fat = val;
                return this;
            }

            public Builder sodium(int val){
                this.sodium = val;
                return this;
            }

            public Builder carbohydrate(int val){
                this.carbohydrate = val;
                return this;
            }

            public NutritionFacts build(){
                return new NutritionFacts(this);
            }
        }
        
        private NutritionFacts(Builder builder){
            servingSize  = builder.servingSize;
            servings     = builder.servings;
            calories     = builder.calories;
            fat          = builder.fat;
            sodium       = builder.sodium;
            carbohydrate = builder.carbohydrate;
        }
    }
    ```

    - NutrutuinFacts 클래스는 불변이며, 모든 매개변수의 기본값들을 한곳에 모아 뒀다
    - 빌더의 세터 메서드들은 빌더 자신을 반환하기 때문데 연쇄적으로 호출 가능
        - 이런 방식을 메서드 호출이 흐르듯 연결된다는 뜻으로 플루언트(fluent) API 혹은 메서드 연쇄(method chaining)라 한다.
    - 이 클래스를 사용하는 클라이언트 코드의 모습

        ```java
        NutritionFacts cocaCola = new NutrutionFacts.Builder(240, 8)
        				.calories(100).sodium(35).carbohydrate(27).build();
        ```

    - 잘못된 매개변수를 일찍 발견하려면 빌더의 생성자와 메서드에서 입력 매개변수를 검사하고 build 메서드가 호출하는 생성자에서 여러 매개변수에 걸친 불변식을 검사하자
        - 불변식(invariant)
            1. 프로그램의 실행 중 일정 구간 동안, 항상 참이 되는 조건(condition)이다
            2. 개발자는 invariant를 보장하기 위해 Assertion 이나 Class invariant 사용
    - 공격에 대비해 이런 불변식을 보장하려면 빌더로부터 매개변수를 복사한 후 해당 객체 필드들도 검사해야함
- 계층적으로 설계된 클래스와 함게 쓰기에 좋다
    - 각 계층의 클래스에 관련 빌더를 멤버로 정의, 추상 클래스는  추상 빌더를, 구체 클래스는 구체 빌더
    - Pizzan.Builder 클래스는 재귀적 타입 한정을 이용하는 제네릭 타입
    - 추상 메서드인 self를 더해 하위 클래스에서는 형변환 하지 않고도 메서드 체이닝 지원
        - self 타입이 없는 자바를 위한 우회 방법을 시뮬레이트한 셀프 타입 관용구라 한다.

    ```java
    public abstract class Pizza{
        public enum Topping { HAM, MUSHROOM, ONION, PEPPER, SAUSAGE }
        final Set<Topping> toppings;

        abstract static class Builder<T extends Builder<T>>{
            EnumSet<Topping> toppings = EnumSet.noneOf(Topping.class);
            public T addTopping(Topping topping){
                toppings.add(Objects.requireNonNull(topping));
                return self();
            }

            abstract Pizza build();

    				// 하위 클래스는 이 메서드를 오버라이딩하여 this를 반환하도록 해야 한다
            protected abstract T self();
        }

        Pizza(Builder<?> builder){
            toppings = builder.toppings.clone();    // 아이템 50 참조
        }
    }
    ```

- 단점
    - 빌더 생성 비용이 문제 될 수도 있다
    - 코드가 장황해서 매개변수가 4개 이상은 되어야 값어치를 한다