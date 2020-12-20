#### Zadanie 4: Books Service

1. W projekcie Library.ConsoleApp stwórz klasę `BooksService`
2. W klasie `BooksService` zaimplementuj takie metody jak: 
   - AddBook():void -> Powinno pobrac od uzytkownika wszystkie dane potrzebne do stworzenia instancji klasy `Book`
   - RemoveBook():void -> Powinno pobrac od uzytkownika tytul ksiazki do usuniecia
   - ListBooks():void -> Ta metoda powinna wyswietlic napis `Tutaj pojawi sie lista ksiazek`
   - ChangeState():void -> Ta metoda powinna pobrac od uzytkownikow tytul ksiazki, której stan ma się zmienic oraz samą zmianę stanu np. -1
Do konwersji string-a na int-a uzyj klasy Convert
```csharp
Convert.ToInt32(Console.ReadLine());
```
3. Przejdz do pliku `Program.cs` w projekcie Library.ConsoleApp
4. Przed pętlą utwórz obiekt klasy `BooksService`
5. W srodku pętli podmien wyswietlanie tekstow na wywołanie odpowiedniej metody z obiektu klasy `BooksService`
6. Uruchom aplikację i sprawdz czy wsyzstko działa poprawnie

#### Zadanie 5: Dodanie repozytorium

1. Otwórz klasę `BooksRepository` znajdującą się w projekcie Library.Persistence
2. Utwórz pole `readonly List<Book>` o nazwie _database
3. Utwórz bezparametroy Konstruktor
4. W konstruktorze dodaj kilka ksiązek, mozesz do tego uzyc ponizszych: 
```csharp
    new Book("Stary człowiek i morze", "Ernest Hemingway", 1986, "AAAA", 10, 19.99m),
    new Book("Komu bije dzwon", "Ernest Hemingway", 1997, "BBBB", 0, 119.99m),
    new Book("Alicja w krainie czarów", "C.K. Lewis", 1998, "CCCC", 53, 39.99m),
    new Book("Opowieści z Narnii", "C.K. Lewis", 1999, "DDDD", 33, 49.99m),
    new Book("Harry Potter", "J.K. Rowling", 2000, "EEEE", 23, 69.99m),
    new Book("Paragraf 22", "Joseph Heller", 2001, "FFFF", 5, 45.99m),
    new Book("Lalka", "Bolesław Prus", 2002, "GGGG", 7, 76.99m),
    new Book("To", "Stephen King", 2003, "HHHH", 2, 12.99m),
    new Book("Idiota", "Fiodor Dostojewski", 1950, "IIII", 89, 25.99m),
    new Book("Mistrz i Małgorzata", "Michaił Bułhakow", 1965, "JJJJ", 41, 36.99m),   
```
5. Dodaj metodę `Insert(Book book): void`, która będzie odpowiedzialna za dodawanie nowej ksiązki do listy
6. Dodaj metodę `GetAll(): List<Book>`, która będzie zwracac wszystkie ksiązki, które znajdują się w repozytorium
7. Dodaj metodę `RemoveByTitle(string title): void`, która będzie kasowac wybraną ksiązke z repozytorium. Aby szybciej namierzyc ksiązke mozesz uzyc ponizszego wyrazenia LINQ
```csharp
.First(x => x.Title == title)
```
8. Dodaj metode `ChangeState(string title, in stateChange)`, która będzie uaktualniac stan w wybranym tytule. Wykorzystaj wyrazenie LINQ z poprzedniego cwiczenia.
9. Przejdz do pliku `BooksService` w projekcie Library.ConsoleApp
10. Utwórz konstruktor, w którym jedynym parametrem będzie obiekt klasy `BooksRepository`
11. Przypisz obiekt klasy `BooksRepository` do pola w klasie o nazwie `_repository`
12. Wykorzystaj obiekt `_repository` w wywołaniach metod klasy `BooksService`
13. Przejdz do pliku `Program.cs` i przed utworzeniem obiektu `BooksService` utworz obiekt klasy `BooksRepository`
14. Przekaz obiekt klasy `BooksRepository` do konstruktora klasy `BooksService`
15. Przetestuj czy mozna dodac nową ksiązke do repozytorium 

#### Zadanie 6: Orders

1. Otwórz projek Library.Domain
2. Stwórz klasę BookOrdered, która powinna zawierac: 
    - Propertis BookId typu int
    - Propertis NumerOrdered typu int
3. Stwórz klasę Order, która powinna zawierac: 
    - Propertis Date typu DateTime
    - propertis BooksOrderedList typu `List<BookOrdered>`
    - Bezparametrowy konstruktor, w którego ciele zastaną wykonane następujące akcję:
        - Ustawienie propertisa Date na wartosc `DateTime.Now`
        - Zainicjalizowanie listy BooksOrderedList pustą listą
    - Metodę ToString (pamiętaj o uzyciu `override`), która wygeneruje ciąg znaków w postaci
        ```
        Order: DataUtworzenia obiektu
        Book: IdKsiazki Count: IloscZamowionych ksiazek
        ```
        w tym celu mozesz uzyc interpolacji stringów
        ```csharp
            string str = "Test";
            str += $"Test: {JakasZmienna} Test2: {JakasZmienna}";
        ```
4. Przejdz do projektu Library.Persistence
5. Utworz klasę `OrdersRepository`
6. Wewnatrz klasy `OrdersRepository` utwórz prywatne pole `database` typu List<Order>, które od razu zainicjalizu pustą listą.
7. Wewnatrz klasy `OrdersRepository` zaimplementuj dwie metody: 
    - `Insert(Order order): void` -> wywołanie tej metody ma dodawac elementy do kolekcji
    - `GetAll(): List<Order>` -> wywołanie tej metody ma zwrócic wszystkie wczesniej dodane order-y
8. Przejdz do projektu Library.ConsoleApp
9. Utwórz klasę `OrderService`
10. W klasie `OrderService` utwórz konstruktor, który będzie przyjmował obiekt klasy `OrdersRepository` jako swój paramter
11. W konstruktorze klasy `OrderService` przypisz obiekt klasy `OrderRepository` do prywatnego pola o nazwie `_orderRepository`
12. W klasie `OrderService` zaimplementuj metodę `PlaceOrder`, która będzie odpowiedzialna za proces składania nowego zamówienia:
    - Utworzenie obiektu klasy `Order`,
    - Wypisanie menu w postaci: 
    ```
        add - dodaj pozycje do zamowienia
        end - zamknij zamowienie
    ```
    - W momencie wpisania komendy `add`, program powinien zapytac uzytkownika o: 
        - Id ksiązki
        - Ilosc 
    - Następnie utworzyc obiekt klasy `BookOrdered` i dodac go do pola `BooksOrderedList` z obiektu `order`
    - Następnie powrócic do menu `add / end` aby bylo mozliwe dodanie wiecej niz tylko jednej pozycji w zamowieniu
    - W momencie wpisania komendy `end` program powinien dodac obiekt `order` do repozytorium
13. W klasie `OrderService` zaimplementuj metodę `ListAll`, której zadaniem będzie wypisanie wszystkich zamowien pobranych z repozytorium
14. Przejdz do pliku `Program.cs` w projekcie Library.ConsoleApp
15. Utwórz obiekt klasy `OrdersRepository` przed główną pętlą programu
16. Utwórz obiekt klasy `OrdersService`, wykorzystując przy tym wczesniej utworzony obiekt klasy `OrdersRepository`
17. Wykorzystaj obiekt klasy `OrdersService` aby podpiąc jego metody do zadan:
    - dodaj zamowienie
    - lista zamowien
18. Uruchom aplikację Library.ConsoleApp w trybie debug-u
19. Przetestuj czy dodawanie zamowien działa poprawnie
20. *Zrób zabezpieczenie aby nie dało się dodac do zamówienia ksiazki, której nie ma na w repozytorium
21. *Zrób zabezpieczenie aby do zamowienia nie dało się dodac więcej ksiązek, niz jest w repozytorium.
 
#### Zadanie 7: Typy Generyczne / Interfejsy

1. Utwórz nowy projekt rodzaju `Console Application`. 
2. Utwórz interfejs IToolKit, który posiada deklarację jednej metody: GetTools(): string[]
3. Utwórz interfejs IParts, który posiada deklarację jednej metody: GetParts(): string[]
4. Utwórz klasę IkeaKit, która posiada jeden parametr generyczny o nazwie `TContents`
5. Dla generycznego parametru klasy IkeaKit dodaj constraintsy tak aby wymusic: 
    - TContents musi implementowac interfejs IToolKit
    - TContents musi implementowac interfejs IParts
    - TContents musi posiadac bezparametrowy konstruktor
6. Wewnatrz klasy IkeaKit zaimplementuj property Title i Colour
7. Oznacz property Title i Colour jako wirtualne: 
    "public virtual string Title { get; set; }"
8. Dodaj metodę GetInventory gdzie w pęli zostaną wypisane wszystkie narzędzia i częsci potrzebne do zlozenia zestawu. 
    "Do tego trzeba bedzie zainicjalizowac obiekt TContents"
9. Utworz klasę Chair
10. Zaimplementuj w klasie Chair interfejsy IToolKit i IParts
    Toolsy mogą zwracac: "Screwdriver", "Allen Key"
    Partsy mogą zwracac: "leg", "leg", "leg", "seat", "back", "bag of screws"
11. Zaimplementu klasę o nazwie Adde (nazwa kszesła z Ikei)
12. Klasa Adde powinna impementowac interfejs IIkeaKit<Chair>
13. W klasie Adde ustaw proper Tittle i Color odpowiednio na wartosci
    "Adde" i "Cyan"
14. Do propertisow klasy Adde dodaj slowo kluczowe override
    "public override string Title"
15. Utwórz blizniaczą do klasy Adde klasę Markus, tylko w tej klasie property mają przyjąc wartosci odpowiednio: "Markus" i "Orange"
16. Następnie wywołaj w funkcji Main ponizszy kod: 
    var chair = new Markus();
	chair.GetInventory();

#### Zadanie 8: Enkapsulacja

1. Otwórz plik Book.cs znajdujący się w projekcie Library.Domain
2. Ze wszystkich property skasuj operator set. 
3. Dla property `ProductsAvailable` do operator set wraz z modyfikatorem prywatnosci private
4. Dodaj do klasy Book nową metodę, która będzie odpowiedzialna za modyfikację stanu magazynowego. Tą metodę nazwij: `ChangeProductsAvailableNumber(int change)`
5. Przejdź do pliku BooksRepository w projekcie Library.Persistence i w metodzie ChangeState zmień ustawienie property na wywołanie metody z klasy Book
6. Przejdź do pliku Order.cs w projekcie Library.Domain
7. W propertach skasuj słowo kluczowe set
8. Przejdź do pliku BookOrdered w projekcie Library.Domain
9. Skasuj słowo klucozwe set w propertach
10. Stwórz konstruktor, który musi zawierac dwa argumenty: Book(int bookId, int numberOrdered)
11. Otwórz plik OrderService znajdujacy sie w projekcie Library.ConsoleApp
12. Zmien w lini gdzie jest dodawana nowa pozycja zamowienia inicjalizacje klasy BookOrdered tak aby była zgodna z tym co przed chwilą było zrobione.     

#### Zadanie 9: Dziedziczenie

1. Przejdz do projektu Library.Domain
2. Utwórz interfejs IModel, ktora ma zaiwerac jeden propertis: int Id { get; set; }
3. Utwórz klasę abstrakcyją BaseModel, która implementuje interfejs IModel
4. Otwórz klasę Book
5. Zastosuj dziedziczenie aby klasa Book dziedziczyla po klasie BaseModel
6. Otworz klasę Order
7. Zastosuj dziedziczenie aby klasa Order dziedziczyla po klasie BaseModel
8. Przejdz do projektu Library.Persistence
9. Utworz klase BaseRepository, ktora bedzie posiadac jeden parametr generyczny o naziwe T
10. Oznacz ze parametr generyczny T musi implementowac interfejs IModel
11. Stworz pole Database typu `List<T>` wraz z zakresem widocznosci protected
12. Stworz pole Counter typu int wraz z zakresem widocznosci protected
13. W klasie BaseRepository utwórz takie metody jak: 
    - Insert(T element): void -> Metoda powinna najpierw ustawic Id elementu na kolejną wartosc Counter-a. A dopiero następnie wstawic element do listy
    - Remove(T element): void -> Metoda powinna usunac wybrany element z listy
    - RemoveById(int id): void -> Metoda powinna usunac wybrany element z listy: 
    Mozemy wykorzystac metode RemoveAll z API obiektu list i za pomoca wyrazenia lambda jasno okreslic ze szukamy elementy o danym id: "x => x.Id == id"
    - GetAll(): `List<T>` -> Metoda zwraca wszystkie elementy, ktore znajduja sie w liscie
    - virtual GetById(int id): T -> Metoda powinna zwracac tylko pojedynczy element odnaleziony po Id, mozemy tutaj zastosowac inna metode pomocniczna z LINQ (SingleOrDefault)
14. Otworz plik BooksRepository
15. Zmien aby klasa BooksRepository dziedziczyla po klasie `BaseRepository<>`
Book
16. Usun niepotrzebne metody z klasy BooksRepository
17. Otworz plik OrdersRepository
18. Zmien aby klasa OrdersRepository dziedziczyla po `BaseRepository<Order>`
19. Usun zbędne elementy z klasy OrdersRepository

#### Zadanie 10: Polimorfizm

1. Utworz nowy projekt typu Aplikacja konsolowa
2. Stworz klasę abstrakcyjną Animal
3. Wewnątrz klasy Animal zaimplementuj: 
   - Propertę wirtualną Weight typu int
   - Propertę wirtualną Meal typu int
   - Metodę wirtualną o nazwie Eat
4. W wwnętrzu metody Eat napisz logike odpowiedzialną za inkrementację propertisu Weight o wartosc propertisu Meal
5. Stwórz klasę Human, która będzie dziedziczyła po klasie Animal
6. W klasie Human utwórz bezparametrowy konstruktor w kótrym ustaw wartosci: 
    - Weight: 75
    - Meal: 3
7. Stwórz klasę Cat, która będzie dziedziczyła po klasie Animal
8. W klasie Cat utwórz bezparametrowy konstruktor w którym ustaw wartosci: 
    - Weight: 5
    - Meal: 1
9. Stwórz klasę Dog, która będzie dziedziczyła po klasie Animal
10. W klasie Dog utwórz bezparametry konstruktor w którym ustaw wartosci
    - Weight: 20
    - Meal: 2
11. W klasie Dog nadpis wirtualną metodą klasy Animal o naziwe Eat
12. Logiką nowej metody Eat w klasie Dog powinno byc dzialanie Weight += Meal / 2;
13. Utwórz obiekt klasy Human przypisując go do zmiennej typu Animal
    - Następnie wywołaj na nim funkcje Eat
    - Wypisz w konsoli propertę Weight tego obiekty
14. Utwórz obiekt klasy Cat przypisując go do zmiennej typu Animal
    - Następnie wywołaj na nim funkcje Eat
    - Wypisz w konsoli propertę Weight tego obiekty
15. Utwórz obiekt klasy Dog przypisując go do zmiennej typu Animal
    - Następnie wywołaj na nim funkcje Eat
    - Wypisz w konsoli propertę Weight tego obiekty