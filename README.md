# КВ_1 Дерево иерархии объектов
Структура связей объектов (элементов) в рамках любой реальной системы имеет древовидную форму. Для моделирования работы системы необходимо аналогично строить древовидную иерархию виртуальных объектов. При этом, необходимо обеспечить, чтобы любой пользовательский объект, созданный в рамках реализации программы, располагался на дереве иерархии объектов.

Дерево иерархии объектов формируется согласно описанию архитектуры системы (древовидного графа) и реализует взаимосвязи между объектами. Объекты в составе иерархии объектов создаются на базе пользовательских классов, определенных на этапе описания метода.

В рамках первого задания реализуется построение иерархии объектов. Так как, на дереве надо расположить все объекты, созданные на базе пользовательских классов, то естественно создать определенный базовый класс, в котором реализовать такую возможность. А далее, все пользовательские классы наследовать от данного базового класса.
Назовем базовый класс cl_base. Он должен содержать свойства для описания древовидного графа. В древовидном графе вершине соответствует объект определенного пользовательского класса. Ребра задаются посредством свойств объекта. Первое свойство должен содержать информацию относительно головного объекта (головной вершины), а второе свойство информацию относительно подчиненных объектов.

Так как дерево иерархии объектов отображает схему их взаимосвязи, то в качестве информации о связанном объекте оптимально использовать адрес данного объекта. Тогда свойства в описании базового класса можно определить следующим образом:
```cpp
cl_base* p_head_object;                    // Указатель на головной объект
vector<cl_base*> subordinate_objects;      // Указатели на подчиненных объектов
```

Исходя из требования, что все пользовательские объекты должны быть расположены на дереве иерархии, то естественно алгоритм привязки к дереву реализовать в конструкторе базового класса. Такой подход обеспечит, что все объекты с момента создания будут располагается на дереве иерархии.
Определим еще требования:
+ Каждый объект должен иметь свое наименование в виде текстовой строки.
+ Подчиненные данному головному объекту объекты должны иметь уникальные наименования.
+ Так как у корневого объекта нет головного, его свойство p_head_object имеет значение нулевого указателя nullptr.

Свойство наименования объекта в описании базового класса определим следующим образом:
```cpp
string s_object_name;      // Наименование объекта
```

Для реализации привязки нового созданного объекта к дереву иерархии объектов определим параметризированный конструктор для базового класса. Конструктору надо будет передать указатель на головной объект и наименование нового объекта. Для второго параметра можно определить значение по умолчанию. Тогда прототип заголовка конструктора будет иметь следующий вид:
> cl_base ( cl_base * p_head_object, string s_object_name = "Base_object" );

К свойствам указателя на головной объект и к наименованию объекта сразу можно присвоить значения параметров. Если значения адреса головного объекта не нулевой указатель, головному объекту надо сообщить, что у него появился новый подчиненный. Это можно сделать посредством добавления указателя текущего (нового созданного) объекта в состав указателей на подчиненные объекты. Тогда алгоритм, реализованный в конструкторе, будет иметь следующий вид:
```cpp
this->p_head_object = p_head_object;                          // Указатель на головной объект
this->s_object_name = s_object_name;                          // Наименование объекта
if (p_head_object)
{
    p_head_object->subordinate_objects.push_back(this);      // Добавление в состав подчиненных головного объекта
}
```

Любое техническое изделие (система), сперва собирается и потом запускается для функционирования. Это естественно. Мы уже отметили, что система является объектом. Согласно жизненного цикла объекта, его первоначально надо сконструировать, а после этого запустить для функционирования. При реализации программы как системы, надо реализовать эти два этапа жизненного цикла. Для определенности, желательно первоначальную реализацию этих этапов строго выделить. Конструктивно поступим следующим образом. Введем понятие объекта приложение. Этот объект будет являться корневым в дереве иерархии объектов системы. Назовем класс этого объекта cl_application. В классе данного объекта определим два метода:

1. Метод реализующий алгоритм первоначального построения дерева иерархии объектов (метод конструирования системы).
2. Метод запуска системы, реализующий основной алгоритм функционирования системы.

Тогда алгоритм основной функции программы будет иметь стандартный вид:
```cpp
int main()
{
     cl_application ob_cl_application(nullptr);    // Создание объекта приложение
     ob_cl_application.build_tree_objects();       // Конструирование системы
     return ob_cl_application.exec_app();          // Запуск системы
}
```

## Постановка задачи
Для организации иерархического построения объектов необходимо разработать базовый класс, который содержит функционал и свойства для построения иерархии объектов. В последующем, в приложениях использовать этот класс как базовый для всех создаваемых классов. Это позволит включать любой объект в состав дерева иерархии объектов.

Каждый объект на дереве иерархии имеет свое место и наименование. Не допускается для одного головного объекта одинаковые наименования в составе подчиненных объектов.

Создать базовый класс со следующими элементами:
+ свойства:
  + наименование объекта (строкового типа);
  + указатель на головной объект для текущего объекта (для корневого объекта значение указателя равно nullptr);
  + динамический массив указателей на объекты, подчиненные к текущему объекту в дереве иерархии.
+ функционал:
  + параметризированный конструктор с параметрами: указатель на объект базового класса, содержащий адрес головного объекта в дереве иерархии; строкового типа, содержащий наименование создаваемого объекта (имеет значение по умолчанию);
  + метод редактирования имени объекта. Один параметр строкового типа, содержит новое наименование объекта. Если нет дубляжа имени подчиненных объектов у головного, то редактирует имя и возвращает «истину», иначе возвращает «ложь»;
  + метод получения имени объекта;
  + метод получения указателя на головной объект текущего объекта;
  + метод вывода наименований объектов в дереве иерархии слева направо и сверху вниз;
  + метод получения указателя на непосредственно подчиненный объект по его имени. Если объект не найден, то возвращает nullptr. Один параметр строкового типа, содержит наименование искомого подчиненного объекта.

Для построения дерева иерархии объектов в качестве корневого объекта используется объект приложение. Класс объекта приложения наследуется от базового класса. Объект приложение реализует следующий функционал:
+ метод построения исходного дерева иерархии объектов (конструирования моделируемой системы);
+ метод запуска приложения (начало функционирования системы, выполнение алгоритма решения задачи).

Написать программу, которая последовательно строит дерево иерархии объектов, слева направо и сверху вниз. Переход на новый уровень происходит только от правого (последнего) объекта предыдущего уровня. Для построения дерева использовать объекты двух производных классов, наследуемых от базового. Исключить создание объекта если его наименование совпадает с именем уже имеющегося подчиненного объекта у предполагаемого головного. Исключить добавление нового объекта, не последнему подчиненному предыдущего уровня.

Построчно, по уровням вывести наименования объектов построенного иерархического дерева.

Основная функция должна иметь следующий вид:
```cpp
int main()
{
     cl_application ob_cl_application(nullptr);    // Создание объекта приложение
     ob_cl_application.build_tree_objects();       // Конструирование системы
     return ob_cl_application.exec_app();          // Запуск системы
}
```

Наименование класса cl_application и идентификатора корневого объекта ob_cl_application могут быть изменены разработчиком.

Все версии курсовой работы имеют такую основную функцию.

При решении задачи необходимо руководствоваться методическим пособием и приложением к методическому пособию

## Входные данные
Первая строка:
> «имя корневого объекта»

Вторая строка и последующие строки:
> «имя головного объекта» «имя подчиненного объекта»

Создается подчиненный объект и добавляется в иерархическое дерево. Если `имя головного объекта` равняется `имени подчиненного объекта`, то новый объект не создается и построение дерева объектов завершается.

Пример ввода:
```
Object_root
Object_root Object_1
Object_root Object_2
Object_root Object_3
Object_3 Object_4
Object_3 Object_5
Object_6 Object_6
```

Дерево объектов, которое будет построено по данному примеру:
```
Object_root
    Object_1
    Object_2
    Object_3
        Object_4
        Object_5
```

## Выходные данные
Первая строка:
`имя корневого объекта`

Вторая строка и последующие строки имена головного и подчиненных объектов очередного уровня разделенных двумя пробелами.
`имя головного объекта` `имя подчиненного объекта`[[`имя подчиненного объекта`]…….]

Пример вывода:
```
Object_root
Object_root  Object_1  Object_2  Object_3
Object_3  Object_4  Object_5
```

---
### Автор - https://vk.com/aptekkkar
