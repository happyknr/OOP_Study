enum \(enumeration\)

* 관련이 있는 상수들의 집합
* 어떤 클래스가 상수만으로 작성되어 있으면 반드시 class로 선언할 필요가 없음 -&gt; 이럴 때 class로 선언된 부분에 enum이라고 선언하면 이 객체는 상수의 집합이라는 것을 명시적으로 나타냄

```
enum Day {  
    MONDAY,TUESDAY, WEDNESDAY, THURSDAY, FRIDAY, SATURDAY, SUNDAY;
}

enum Month {  
    JANUARY, FEBRUARY, MARCH, APRIL, MAY, JUNE, JULY, 
    AUGUST, SEPTEMBER, OCTOBER, NOVEMBER, DECEMBER;
}

public class EnumExample {

    public static void main(String[] args) {        

        Day day = Day.MONDAY;

        switch (day) {
        case MONDAY:
            System.out.println("월요일입니다.");
            break;
        case TUESDAY:
            System.out.println("화요일입니다.");
            break;
        case WEDNESDAY:
            System.out.println("수요일입니다.");
            break;

            . . .
        }
    }
}
```



