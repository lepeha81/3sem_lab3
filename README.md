# 3sem_lab3
## Homework
### Задание 1
Создайте структуру Vector с тремя полями X, Y и Z. 
Для созданной структуры переопределите операторы сложения векторов, умножения векторов, умножения вектора на число, а также логические операторы. Для логических операторов используйте сравнение по длине от начала координат.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
struct Vector
{
    private double x;
    private double y;
    private double z;
    public Vector (double x, double y, double z)
    {
        this.x = x;
        this.y = y;
        this.z = z;
    }
    public double X 
    {
        get { return x; }
        set { x = value; }
    }
    public double Y
    {
        get { return y; }
        set { y = value; }
    }
    public double Z
    {
        get { return z; }
        set { z = value; }
    }
    public static Vector operator +(Vector one, Vector two)//public static, так как перегружаемый оператор будет использоваться для всех объектов данного класса
    {
        return new Vector(one.X + two.X, one.Y + two.Y, one.Z + two.Z);
    }
    public static double operator *(Vector one, Vector two)
    {
        return one.X * two.X + one.Y * two.Y + one.Z * two.Z;
    }
    public static Vector operator*(Vector one, double e)
    {
        return new Vector(one.X * e, one.Y * e, one.Z * e);
    }
    public static bool operator <(Vector one, Vector two)
    {
        double length1 = Math.Sqrt(one.X * one.X + one.Y * one.Y + one.Z * one.Z);
        double length2 = Math.Sqrt(two.X * two.X + two.Y * two.Y + two.Z * two.Z);
        return length1 < length2;
    }

    public static bool operator >(Vector one, Vector two)
    {
        double length1 = Math.Sqrt(one.X * one.X + one.Y * one.Y + one.Z * one.Z);
        double length2 = Math.Sqrt(two.X * two.X + two.Y * two.Y + two.Z * two.Z);
        return length1 > length2;
    }

    public static bool operator ==(Vector one, Vector two)
    {
        return one.X == two.X && one.Y == two.Y && one.Z == two.Z;//и
    }

    public static bool operator !=(Vector one, Vector two)
    {
        return !(one == two);
    }
}
class Program
{
    static void Main()
    {
        Vector v1 = new Vector(10.0, 20.0, 30.0);
        Vector v2 = new Vector(40.0, 50.0, 60.0);

        Vector sum = v1 + v2;
        double itog = v1 * v2;
        Vector nascalyar = v1 * 2.0;
        bool sravnenie = v1 < v2;

        Console.WriteLine($"Sum: ({sum.X}, {sum.Y}, {sum.Z})");
        Console.WriteLine($"Scalyarnoe proisvedenie: {itog}");
        Console.WriteLine($"nascalyar: ({nascalyar.X}, {nascalyar.Y}, {nascalyar.Z})");
        Console.WriteLine($"sravnenie: {sravnenie}");

        Vector v3 = new Vector(1.0, 2.0, 3.0);
        bool yyyy = v1 == v3;
        Console.WriteLine($"v1 == v3: {yyyy}");
    }
}
```

### Задание 2
Создайте класс Car со свойствами Name, Engine, MaxSpeed. Переопределите оператор ToString() таким образом, чтобы он возвращал название машины(Name). Реализуйте возможность сравнения объектов Car, реализовав интерфейс IEquatable<Car>. 
Создайте класс CarsCatalog, содержащий коллекцию машин – элементов типа Car и переопределите для него индексатор таким образом, чтобы он возвращал строку с названием машины и типом двигателя.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

public class Car
{
    private string name;
    private int engine;//двигатель
    private double maxSpeed;
    public string Name
    {
        get { return name; }
        set { name = value; }
    }

    public int Engine
    {
        get { return engine; }
        set { engine = value; }
    }

    public double MaxSpeed
    {
        get { return maxSpeed; }
        set { maxSpeed = value; }
    }

    public Car(string name, int engine, double maxSpeed)
    {
        Name = name;
        Engine = engine;
        MaxSpeed = maxSpeed;
    }

    public override string ToString()
    {
        return Name;
    }
}

class CarComparer : IEquatable<Car>// Объявление класса CarComparer, который реализует интерфейс IEquatable<Car>
{//Этот класс будет использоваться для сравнения двух объектов типа Car на равенство.
    public bool Equals(Car car1, Car car2)//Этот метод сравнивает два объекта типа Car на равенство, сравнивая их имя, мощность двигателя и максимальную скорость.
    {
        if (car1 == null || car2 == null)
            return false;

        return car1.Name == car2.Name && car1.Engine == car2.Engine && car1.MaxSpeed == car2.MaxSpeed;
    }

    public bool Equals(Car? other)//проверка на нулл
    {
        throw new NotImplementedException();
    }
}

class CarsCatalog
{
    private List<Car> cars;

    public CarsCatalog()
    {
        cars = new List<Car>();
    }

    public void AddCar(Car car)
    {
        cars.Add(car);//добавление
    }

    public Car this[int index]//индексатор
    {
        get
        {
            if (index >= 0 && index < cars.Count)
                return cars[index];
            else
                throw new IndexOutOfRangeException("Индекс находится вне диапазона");
        }
    }
}

class lab032
{
    static void Main()
    {
        Car car1 = new Car("BMW", 700, 700);
        Car car2 = new Car("Mersedes", 600, 600);

        CarsCatalog catalog = new CarsCatalog();
        catalog.AddCar(car1);
        catalog.AddCar(car2);

        Console.WriteLine("Car 1: " + catalog[0].Name + " - " + catalog[0].Engine);
        Console.WriteLine("Car 2: " + catalog[1].Name + " - " + catalog[1].Engine);

        if (Equals(car1, car2))
            Console.WriteLine($"\n{car1} i {car2} ravni");
        else
            Console.WriteLine($"\n{car1} i {car2} ne ravni");
    }
}

```
### Задание 3
Создайте базовый класс Currency со свойством Value. Создайте 3 производных от Currency класса – CurrencyUSD, CurrencyEUR и CurrencyRUB со свойствами, соответствующими обменному курсу. В каждом из производных классов переопределите операторы преобразования типов таким образом, чтобы можно было явно или неявно преобразовать одну валюту в другую по курсу, заданному пользователем при запуске программы.
```
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
public abstract class Currency
{
    protected double value;
}
class CurrencyUSD : Currency
{
    public CurrencyUSD(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyEUR(CurrencyUSD val)
    {//public static implicit operator Тип_в_который_надо_преобразовать(исходный_тип param)
        return new CurrencyEUR(val.value / CurrencyEUR.ExChange);
    }
    public static implicit operator CurrencyRUB(CurrencyUSD val)
    {
        return new CurrencyRUB(val.value / CurrencyRUB.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
}
class CurrencyEUR : Currency
{
    public static double ExChange { get; set; }

    public CurrencyEUR(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyRUB(CurrencyEUR val)
    {
        return new CurrencyRUB(val.value * CurrencyEUR.ExChange / CurrencyRUB.ExChange);
    }
    public static implicit operator CurrencyUSD(CurrencyEUR val)
    {
        return new CurrencyUSD(val.value * CurrencyEUR.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
}
class CurrencyRUB : Currency
{
    public static double ExChange { get; set; }
    public CurrencyRUB(double value)
    {
        this.value = value;
    }
    public static implicit operator CurrencyUSD(CurrencyRUB val)
    {
        return new CurrencyUSD(val.value * CurrencyRUB.ExChange);
    }
    public static implicit operator CurrencyEUR(CurrencyRUB val)
    {
        return new CurrencyEUR(val.value * CurrencyRUB.ExChange / CurrencyEUR.ExChange);
    }
    public double Value
    {
        get { return this.value; }
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        CurrencyEUR.ExChange = 1.06;//обменный курс между долларом США (USD) и евро (Euro)
        CurrencyRUB.ExChange = 0.010265;//(рубль в доларрах)

        CurrencyUSD cur = new CurrencyUSD(100);
        CurrencyEUR eur = cur;
        Console.WriteLine($"It's {eur.Value} EUR");
        CurrencyRUB rub = eur;
        Console.WriteLine($"It's {rub.Value} RUB");
    }
}

```
