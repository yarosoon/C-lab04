# Csharp_lab04
## Цели работы:
  1. Научиться синтаксису и принципам работы с массивами средствами языка C#.
  2. Научиться реализовывать интерфейсы IComparer<T> и IEnumerator<T>.
  3. Получить практические навыки работы с оператором yield.

## Задание №1
Создайте класс MyMatrix, представляющий матрицу m на n.
Создайте конструктор, принимающий число строк и столбцов, заполняющий матрицу случайными числами в диапазоне, который пользователь вводит при запуске программы.
Определите операторы сложения, вычитания и умножения матриц, а также умножения и деления матрицы на число.
Создайте пользовательский индексатор матрицы для доступа к элементам матрицы по номеру строки и столбца.

## Задание №2
Создайте класс Car с тремя авто-свойствами: Name, ProductionYear и MaxSpeed, соответствующими названию, году выпуска и максимальной скорости соответственно.
Создайте класс CarComparer : IComparer<Car> и реализуйте метод Compare таким образом, чтобы можно было сортировать массив элементов Car по названию, году выпуска или максимальной скорости по выбору.
Создайте массив элементов Car и продемонстрируйте сортировку различными способами.

## Задание №3
Используйте класс Car из задания №2, на его основе создайте класс CarCatalog, содержащий массив элементов типа Car. 
Для класса CarCatalog реализуйте возможность итерации по элементам массива Car с помощью оператора foreach различными способами: 
Прямой проход с первого элемента до последнего.
Обратный проход от последнего к первому.
Проход по элементам массива с фильтром по году выпуска.
Проход по элементам массива с фильтром по максимальной скорости.

Примечание: для выполнения задания необходимо реализовать различные итераторы, используя конструкцию yield return. Для п.3 и 4, итератор должен принимать год выпуска и скорость как параметр, чтобы возвращать только те элементы коллекции, которые удовлетворяют условию.
## Код задания №1
    using System;
    
    public class MyMatrix
    {
        private int[,] data; // Двумерный массив для хранения элементов матрицы
        private int rows; // Количество строк в матрице
        private int columns; // Количество столбцов в матрице
    
        // Конструктор для инициализации матрицы с заданными размерами и заполнения случайными числами
        public MyMatrix(int rows, int columns, int minValue, int maxValue)
        {
            this.rows = rows; // Установка количества строк
            this.columns = columns; // Установка количества столбцов
            data = new int[rows, columns]; // Инициализация двумерного массива
            Random rand = new Random(); // Генератор случайных чисел
    
            // Заполнение матрицы случайными числами в заданном диапазоне
            for (int i = 0; i < rows; i++)
            {
                for (int j = 0; j < columns; j++)
                {
                    data[i, j] = rand.Next(minValue, maxValue); // Генерация случайного числа
                }
            }
        }
    
        // Индексатор для доступа к элементам матрицы по индексу строки и столбца
        public int this[int row, int column]
        {
            get { return data[row, column]; } // Возврат элемента матрицы
            set { data[row, column] = value; } // Установка значения элемента матрицы
        }
    
        // Перегрузка оператора сложения для суммирования двух матриц
        public static MyMatrix operator +(MyMatrix a, MyMatrix b)
        {
            // Проверка на соответствие размеров матриц
            if (a.rows != b.rows || a.columns != b.columns)
            {
                Console.WriteLine("!!!ERROR!!! Matrices must have the same dimensions to add."); // Вывод ошибки
                return null; // Возврат null в случае ошибки
            }
    
            // Создание новой матрицы для результата
            MyMatrix result = new MyMatrix(a.rows, a.columns, 0, 1);
            for (int i = 0; i < a.rows; i++)
            {
                for (int j = 0; j < a.columns; j++)
                {
                    result[i, j] = a[i, j] + b[i, j]; // Суммирование элементов
                }
            }
            return result; // Возврат результирующей матрицы
        }
    
        // Перегрузка оператора вычитания для вычитания одной матрицы из другой
        public static MyMatrix operator -(MyMatrix a, MyMatrix b)
        {
            // Проверка на соответствие размеров матриц
            if (a.rows != b.rows || a.columns != b.columns)
            {
                Console.WriteLine("!!!ERROR!!! Matrices must have the same dimensions to subtract."); // Вывод ошибки
                return null; // Возврат null в случае ошибки
            }
    
            // Создание новой матрицы для результата
            MyMatrix result = new MyMatrix(a.rows, a.columns, 0, 1);
            for (int i = 0; i < a.rows; i++)
            {
                for (int j = 0; j < a.columns; j++)
                {
                    result[i, j] = a[i, j] - b[i, j]; // Вычитание элементов
                }
            }
            return result; // Возврат результирующей матрицы
        }
    
        // Перегрузка оператора умножения матриц
        public static MyMatrix operator *(MyMatrix a, MyMatrix b)
        {
            // Проверка на соответствие размеров для умножения
            if (a.columns != b.rows)
            {
                Console.WriteLine("!!!ERROR!!! Number of columns in the first matrix must equal the number of rows in the second."); // Вывод ошибки
                return null; // Возврат null в случае ошибки
            }
    
            // Создание новой матрицы для результата
            MyMatrix result = new MyMatrix(a.rows, b.columns, 0, 1);
            for (int i = 0; i < a.rows; i++)
            {
                for (int j = 0; j < b.columns; j++)
                {
                    // Умножение матриц
                    for (int k = 0; k < a.columns; k++)
                    {
                        result[i, j] += a[i, k] * b[k, j]; // Суммирование произведений для каждой ячейки результата
                    }
                }
            }
            return result; // Возврат результирующей матрицы
        }
    
        // Перегрузка оператора умножения матрицы на число
        public static MyMatrix operator *(MyMatrix a, int scalar)
        {
            // Создание новой матрицы для результата
            MyMatrix result = new MyMatrix(a.rows, a.columns, 0, 1);
            for (int i = 0; i < a.rows; i++)
            {
                for (int j = 0; j < a.columns; j++)
                {
                    result[i, j] = a[i, j] * scalar; // Умножение каждого элемента на скаляр
                }
            }
            return result; // Возврат результирующей матрицы
        }
    
        // Перегрузка оператора деления матрицы на число
        public static MyMatrix operator /(MyMatrix a, int scalar)
        {
            // Проверка деления на ноль
            if (scalar == 0)
            {
                Console.WriteLine("!!!ERROR!!! Cannot divide by zero."); // Вывод ошибки
                return null; // Возврат null в случае ошибки
            }
    
            // Создание новой матрицы для результата
            MyMatrix result = new MyMatrix(a.rows, a.columns, 0, 1);
            for (int i = 0; i < a.rows; i++)
            {
                for (int j = 0; j < a.columns; j++)
                {
                    result[i, j] = a[i, j] / scalar; // Деление каждого элемента на скаляр
                }
            }
            return result; // Возврат результирующей матрицы
        }
    
        // Метод для вывода матрицы на консоль
        public void Print()
        {
            for (int i = 0; i < rows; i++)
            {
                for (int j = 0; j < columns; j++)
                {
                    Console.Write(data[i, j] + "\t"); // Вывод элемента с табуляцией
                }
                Console.WriteLine(); // Переход на следующую строку
            }
        }
    }
    
    // Пример использования класса MyMatrix
    public class Program
    {
        public static void Main()
        {
            // Ввод размеров матрицы
            Console.WriteLine("Введите количество строк и столбцов матрицы:");
            int rows = Convert.ToInt32(Console.ReadLine());
            int columns = Convert.ToInt32(Console.ReadLine()); 
    
            // Ввод диапазона для случайных чисел
            Console.WriteLine("Введите минимальное и максимальное значения для заполнения матрицы:");
            int minValue = Convert.ToInt32(Console.ReadLine()); 
            int maxValue = Convert.ToInt32(Console.ReadLine());
    
            // Создание двух матриц с случайными числами
            MyMatrix matrixA = new MyMatrix(rows, columns, minValue, maxValue);
            MyMatrix matrixB = new MyMatrix(rows, columns, minValue, maxValue);
    
            // Вывод матриц A и B
            Console.WriteLine("Матрица A:");
            matrixA.Print();
            Console.WriteLine("Матрица B:");
            matrixB.Print();
    
            // Сложение матриц A и B
            MyMatrix sum = matrixA + matrixB;
            if (sum != null) // Проверка на null
            {
                Console.WriteLine("Сумма A и B:");
                sum.Print();
            }
    
            // Вычитание матриц A и B
            MyMatrix difference = matrixA - matrixB;
            if (difference != null) // Проверка на null
            {
                Console.WriteLine("Разность A и B:");
                difference.Print();
            }
    
            // Пример умножения матриц
            MyMatrix matrixC = new MyMatrix(columns, rows, minValue, maxValue); // Размеры должны быть совместимы для умножения
            Console.WriteLine("Матрица C:");
            matrixC.Print();
    
            // Умножение матриц A и C
            MyMatrix product = matrixA * matrixC;
            if (product != null) // Проверка на null
            {
                Console.WriteLine("Произведение A и C:");
                product.Print();
            }
    
            // Умножение и деление матрицы A на число
            int scalar = 2;
            MyMatrix multiplied = matrixA * scalar; // Умножение на скаляр
            Console.WriteLine($"Матрица A, умноженная на {scalar}:");
            multiplied.Print();
    
            MyMatrix divided = matrixA / scalar; // Деление на скаляр
            Console.WriteLine($"Матрица A, деленная на {scalar}:");
            divided.Print();
        }
    }
## Код задания №2
    using System;
    using System.Collections.Generic;
    
    // Класс Car представляет автомобиль с тремя свойствами: название, год выпуска и максимальная скорость
    public class Car
    {
        public string Name { get; set; } // Название автомобиля
        public int ProductionYear { get; set; } // Год выпуска автомобиля
        public int MaxSpeed { get; set; } // Максимальная скорость автомобиля
    
        // Конструктор для инициализации свойств
        public Car(string name, int productionYear, int maxSpeed)
        {
            Name = name;
            ProductionYear = productionYear;
            MaxSpeed = maxSpeed;
        }
    
        // Переопределение метода ToString для удобного отображения информации об автомобиле
        public override string ToString()
        {
            return $"Name: {Name}, Year: {ProductionYear}, Max Speed: {MaxSpeed}";
        }
    }
    
    // Класс CarComparer реализует интерфейс IComparer<Car> для сортировки автомобилей
    public class CarComparer : IComparer<Car> // Интерфейс, для сравнения
    {
        // Перечисление, определяющее критерии сортировки
        public enum SortCriteria // Определяем три возможных критерия сортировки для автомобилей
        {
            ByName,            // Сортировка по названию
            ByProductionYear,  // Сортировка по году выпуска
            ByMaxSpeed         // Сортировка по максимальной скорости
        }
    
        private SortCriteria sortCriteria; // Переменная для хранения текущего критерия сортировки
    
        // Конструктор для установки критерия сортировки
        public CarComparer(SortCriteria criteria)
        {
            sortCriteria = criteria;
        }
    
        // Метод Compare для сравнения двух автомобилей в зависимости от выбранного критерия
        public int Compare(Car x, Car y)
        {
            switch (sortCriteria)
            {
                case SortCriteria.ByName:
                    return x.Name.CompareTo(y.Name); // Сравнение по названию
                case SortCriteria.ByProductionYear:
                    return x.ProductionYear.CompareTo(y.ProductionYear); // Сравнение по году выпуска
                case SortCriteria.ByMaxSpeed:
                    return x.MaxSpeed.CompareTo(y.MaxSpeed); // Сравнение по максимальной скорости
                default:
                    Console.WriteLine("!!!ERROR!!! Invalid sort criteria"); // Вывод сообщения об ошибке
                    return 0; // Возвращаем 0, чтобы не прерывать сортировку
            }
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            Car[] cars = {
                new Car("Ferrari", 2015, 250),
                new Car("Skoda", 2018, 200),
                new Car("BMW", 2023, 240),
                new Car("Audi", 2020, 220)
            };
    
            Console.WriteLine("Original array:");
            // Выводим исходный массив
            for (int i = 0; i < cars.Length; i++)
            {
                Console.WriteLine(cars[i]);
            }
    
            // Сортировка по имени
            Array.Sort(cars, new CarComparer(CarComparer.SortCriteria.ByName));
            Console.WriteLine("\nSorted by Name:");
            // Выводим отсортированный массив
            for (int i = 0; i < cars.Length; i++)
            {
                Console.WriteLine(cars[i]); 
            }
    
            // Сортировка по году выпуска
            Array.Sort(cars, new CarComparer(CarComparer.SortCriteria.ByProductionYear));
            Console.WriteLine("\nSorted by Production Year:");
            // Выводим отсортированный массив
            for (int i = 0; i < cars.Length; i++)
            {
                Console.WriteLine(cars[i]); 
            }
    
            // Сортировка по максимальной скорости
            Array.Sort(cars, new CarComparer(CarComparer.SortCriteria.ByMaxSpeed));
            Console.WriteLine("\nSorted by Max Speed:");
            // Выводим отсортированный массив
            for (int i = 0; i < cars.Length; i++)
            {
                Console.WriteLine(cars[i]); 
            }
        }
    }
## Код задания №3
    using System;
    using System.Collections;
    using System.Collections.Generic;
    
    // Класс Car представляет автомобиль с тремя свойствами: название, год выпуска и максимальная скорость
    public class Car
    {
        public string Name { get; set; } // Название автомобиля
        public int ProductionYear { get; set; } // Год выпуска автомобиля
        public int MaxSpeed { get; set; } // Максимальная скорость автомобиля
    
        // Конструктор для инициализации свойств
        public Car(string name, int productionYear, int maxSpeed)
        {
            Name = name;
            ProductionYear = productionYear;
            MaxSpeed = maxSpeed;
        }
    
        // Переопределение метода ToString для удобного отображения информации об автомобиле
        public override string ToString()
        {
            return $"Name: {Name}, Year: {ProductionYear}, Max Speed: {MaxSpeed}";
        }
    }
    
    // Класс CarCatalog для хранения массива автомобилей и реализации различных итераторов
    public class CarCatalog : IEnumerable<Car> // Интерфейс, который позволяет перебирать коллекцию
    {
        private Car[] cars; // Массив автомобилей
    
        // Конструктор для инициализации массива автомобилей
        public CarCatalog(Car[] cars)
        {
            this.cars = cars;
        }
    
        // Итератор для прямого прохода по массиву
        public IEnumerator<Car> GetEnumerator()
        {
            for (int i = 0; i < cars.Length; i++)
            {
                yield return cars[i]; // yield - метод, который может возвращать последовательность значений
            }
        }
    
        // Итератор для обратного прохода по массиву
        public IEnumerable<Car> ReverseIterator()
        {
            for (int i = cars.Length - 1; i >= 0; i--)
            {
                yield return cars[i]; // yield - метод, который может возвращать последовательность значений
            }
        }
    
        // Итератор с фильтром по году выпуска
        public IEnumerable<Car> FilterByYear(int year)
        {
            foreach (var car in cars)
            {
                if (car.ProductionYear == year)
                {
                    yield return car; // yield - метод, который может возвращать последовательность значений
                }
            }
        }
    
        // Итератор с фильтром по максимальной скорости
        public IEnumerable<Car> FilterByMaxSpeed(int maxSpeed)
        {
            foreach (var car in cars)
            {
                if (car.MaxSpeed == maxSpeed)
                {
                    yield return car; // yield - метод, который может возвращать последовательность значений
                }
            }
        }
    
        // Реализация IEnumerable для поддержки foreach (метод для использования класса CarCatalog в foreach циклах)
        IEnumerator IEnumerable.GetEnumerator()
        {
            return GetEnumerator();
        }
    }
    
    class Program
    {
        static void Main(string[] args)
        {
            Car[] cars = {
                new Car("Ferrari", 2015, 250),
                new Car("Skoda", 2018, 200),
                new Car("BMW", 2023, 240),
                new Car("Audi", 2020, 220)
            };
    
            // Создаем каталог автомобилей
            CarCatalog carCatalog = new CarCatalog(cars);
    
            // Прямой проход
            Console.WriteLine("Direct iteration using foreach:");
            foreach (var car in carCatalog)
            {
                Console.WriteLine(car);
            }
            Console.WriteLine("\nDirect iteration using GetEnumerator():");
            IEnumerator<Car> enumerator = carCatalog.GetEnumerator(); // Интерфейс для перебора + метод для поочередного перебора элементов carCatalog
            while (enumerator.MoveNext()) // Movenext() - метод, который передвигает курсор на следующий элемент коллекции
            {
                Console.WriteLine(enumerator.Current); // Свойство, которое возвращает текущий элемент коллекции на котором сейчас находится enumerator
            }
    
            // Обратный проход
            Console.WriteLine("\nReverse iteration:");
            foreach (var car in carCatalog.ReverseIterator())
            {
                Console.WriteLine(car);
            }
    
            // Проход с фильтром по году выпуска
            Console.WriteLine("\nFilter by Production Year (2018):");
            foreach (var car in carCatalog.FilterByYear(2018))
            {
                Console.WriteLine(car);
            }
    
            // Проход с фильтром по максимальной скорости
            Console.WriteLine("\nFilter by Max Speed (250):");
            foreach (var car in carCatalog.FilterByMaxSpeed(250))
            {
                Console.WriteLine(car);
            }
        }
    }
C# lab04
