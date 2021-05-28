# Едносвързан списък и итератори
## Едносвързан списък
### Едносвързания списък представлява множество от кутийки в паметта (нашата информация, която искаме да запазим) свързани чрез указатели, така че да знаем след на всяка кутия следващата. Доста е подобен на вектора, логически. Имплементацията е по - различна и употребата му може да е по - рядка, но всичко зависи от проблема, който решаваме. 

<br>

### **Пример за предимство:** Ако имаме два едносвързани списъка може да кажем, че на последната кутийка от първия списък следващата е първата от втория списък, така първия списък мигновено става по - дълъг и свързваме втория за него за отрицателно време.

<br>

### **Пример за недостатък:** Не може да достъпим елементите чрез директна индексация.

## Шаблон за имплементация.
``` c++
template <typename T>
class LList {
private:
    // нашата кутийка, която има data (какво има в нея) 
    // и указател към Node (коя е следващата кутия)
    struct Node { 
        T data;
        Node* next;
    }

    Node* first; // указател към първия елемент
    void copy(const LList& other);
    void erase();
public:
    // голяма четворка
    LList();
    LList(const LList& other);
    LList& operator=(const LList& other);
    ~LList();
};
```

## Итератор
### Итераторът е design pattern, които ни позволява да итерираме през елементите на даден контейнер без индекси и без сложни цикли. Накратко, когато имплементираме итератор за даден контейнер можем да кажем for (int element : vector), т.е завърти цикъла докато не минем всеки елемент на вектора. 
### За да постигнем това са ни нужни няколко неща
- class Iterator (), който е в public частта на контейнер класа (в повечето случаи е public)
- в private частта на Iterator-a данни, които ще ни пазят информация до кой елемент сме итерирали (current element)
- в public частта на Iterator класа, задължителнот трябва да имаме:
    - конструктор за инициализиране на private член данните.
    - T& оператор *, който връща информацията съхранена в current element
    - void оператор ++, който прави current element следващия елемент в контейнера.
    - bool operator !=(const Iterator& it) - начин за различаване на два итератора тъй като, когато итерираме елементите на контейнер искаме да итерираме докато не стигнем до някъде, това докато можем само да го определим с operator !=
- отделно в контейнер класа трябва да имаме два метода
    - Iterator begin(); връща откъде трябва да започне итератора
    - Iterator end(); връща къде трябва да приключи итератора.

## Шаблонна имплементация на Итератор в едносвързан списък
``` c++
template <typename T>
class LList {
public:
    // голяма четворка
    LList();
    // LList(const LList& other);
    // LList& operator=(const LList& other);
    // ~LList();

    void addLast(const T& newElement);
    void addFirst(const T& newElement);
    void removeLast();
    void print() const;

private:
    struct Node { // нашата кутийка, която има data (какво има в нея) и указател към Node (коя е следващата кутия)
        T data;
        Node* next;
    };

    Node* first; // указател към първия елемент
    void copy(const LList& other);
    void erase();

public:
    class Iterator {
        public:
            Iterator();
            Iterator(Node* current);
            bool operator !=(const Iterator& it);
            void operator++();
            T& operator* ();
        private:
            Node* current;
    };

public:
    Iterator begin();
    Iterator end();
};
```