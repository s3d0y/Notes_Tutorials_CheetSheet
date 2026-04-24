#программирование #programming

- `include <название файла>`  - директива. Вставляет код из указанного файла в текущий файл

например
```C
#include <stdio.h> 
//standard input/output header — стандартный заголовочный файл ввода-вывода
```

- `тип название (параметры) {тело функции и возвращаемое значение}  - описание функуции

``` c
#include <stdio.h>
#include <stdlib.h>

# объявить о том, что в программе имеется функция
int kelvin_to_celcius(int); 
 
int main(int argc, char **argv)
{
    int num = atoi(argv[1]);
#из функции main обратиться к функци kelvin_to_celsius и передать в ее параметр значение num, получить итог работы функции, продолжитьб выполнение кода
    printf("%d\n", kelvin_to_celcius(num)); 
    return 0;
}

#при обращении к функции получить передаваемое значнение в параметр - переменную kelvin
int kelvin_to_celcius(int kelvin){ 
    int celsius; 
    celsius=kelvin - 273;
#исполнить код в функции
#вернуть итог работы кода фукнции туда откуда обратились  
    return celsius; 
}
```


- `int main(void) {}` - обязательная стартовая функция в C

```C
int main(void) {
	
	return 0;
}
```
- `while (условие) {тело цикла}` - цикл while 
`
Например: 
	
``` C 
int main (void) {

	float mile, kilometre;
	float min, max, step;
    
	max = 300;
	kilometre = min;
	step = 20.0;
    
	while (kilometre<= max){
	    mile=kilometre *0.621;  
	    printf("%3.2f : %3.2f\n", kilometre, mile);
	    kilometre = kilometre + step;
	
	    }
}
```

- `for (инициализация счетчика; условие выполнения; шаг счетчика){тело цикла}` - цикл for. если тело цикла в одну строку и на этом все заканчивается, то фигурные скобки не обязательны

``` C 
#include <stdio.h>

int main(void) {

    float fahr, celcius;
    float lower, upper, step;
  
    for (fahr =0; fahr <= 300; fahr = fahr+ 20)
        printf("%3.2f :  %3.2f\n", fahr, (5.0/9.0)*(fahr-32) );
}
```

- `if (условие) { если условие истина волняем этот код} else { инае выполняем этот код} 

``` c
#include <stdio.h>

void fizzbuzz(int limit);

int main(void)
{
    fizzbuzz(20);
    return 0;
}

void fizzbuzz(int limit)
{
    int i;

    for (i=1; i<=limit; ++i) {
        if (!(i % 15))
            printf("FizzBuzz");
        else if (!(i % 3))
            printf("Fizz");
        else if (!(i % 5))
            printf("Buzz");
        else
            printf("%d", i);

        if (i < limit)
            printf(" ");
    }
}
```

- `swithc (аргумент) {case значение: break; default:}` - цикл
при получении аргумента он сравнивается с тем что есть в case. при совпадении case срабатывает. Если ничего не совпало сработает default

break служит для прерывания и выхода их функции, т.к. даже после "удачного" case  код продолжит выполнятся в функции 


``` c
#include <stdio.h>
#include <stdlib.h>

void number_printer(int number);

int main(int argc, char **argv)
{
  int num = atoi(argv[1]);
  number_printer(num);
  return 0;
}

void number_printer(int number)
{
    char text;
    switch (number)
    {
        case 0:
            printf ("Zero");
            break;
        case 1:
            printf ("One");
            break;
        case 2:
            printf ("Two");
            break;
        case 3:
            printf ("Three");
            break;
        default:
        printf ("%d", number);

    }

}



```

	- `int arr[]` -  массив

``` c

int main(void)
 {
    int arr[4];
    arr [0] = 32;
    arr [1] = 12;
    arr [2] = 42;
    arr [3] = 99;
 
    int i, sum;
    sum = 0;

    for (i=0; i < 4; ++i)
        sum = sum + arr[i];
 
    printf("%d\n", sum);
    return 0;
 }
```

- `=`  - [[оператор присваивания]]

- `+=`, `-=`, `*=`, `/=`, `%=`, x++, ++x -  [[короткие арифметические конструкции]] 


[[логические операторы C]]

[[округление]]
