#### Zadanie 11: Podsumowanie / Refactor aplikacji

1. Przejdz do projektu Library.ConsoleApp
2. Utwórz interfejs IHandler, który ma zawierac jedną metodę Handle():void
3. Utwórz interfejs ICommand, któr ma zawierac dwie property: 
    - string Name { get; }
    - string Description { get; }
4. Utwórz klasę Handler, która ma implementowac interfejs IHandler
5. Utwórz w klasie Handler konstruktor z jednym parametrem. Ten paramter ma byc typu Action i nazywac sie logic
6. Zapisz przekazany paramter do prywatnego pola w klasie o naziwe _logic
7. Wewnatz metody Handle uruchom wczesniej otrzymaną funckę _logic (ex: "_logic()")
8. Utwórz klasę Command, która ma implementowac interfejs ICommand
9. Klasa Command ma posiadac konstruktor, ktory przyjmuje dwa parametry: 
    - string name
    - string description
10. Utwórz klasę Procesor
11. Klasa procesor ma zawierac prywatne pole typu Dictionary<ICommand, IHandler> o nazwie "_actions". Te pole powinno byc od razu zaincjalizowane za pomoca "new Dictionary<ICommand, IHandler>()"
12. Klasa powinna miec prywatna statyczna metodę o nazwie "AfterCommand", w której powinien znaleźc się kod odpowiedzialny za wyswietlenie komunikatu: "Nacisnij dowolny przycisk aby kontynouwac." A następnie metoda ta powinna pobrac dowolny znak z klawiatury za pomoca "Console.ReadKey"
13. Zaimplementuj w klasie metode o nazwie "RegisterNewAction(ICommand, IHandler): void", która powinna dodac nowy command i nowy handler do slownika.
14. Zaimplementuj w klasie metodę o nazwie GetAllCommandsWithDescription(): string
Ciało funkcji mozesz skopiowac: 
    var sb = new StringBuilder();

    foreach (var (command, handler) in _actions)
    {
        sb.Append($"{command.Name} - {command.Description} \n");
    }

    return sb.ToString();
    
    +++++++++++++++++++++++++++++++++++++++++++++++++++++++++
15. Zaimplementuj metodę HandlerCommand(string command): void
16. Przejdz do pliku Program.cs
17. Ponizej utworzenia serwisów i repozytorium, stwórz obiekt klasy Processor
18. Podefiniuj akcje procesora wykorzystując metodę RegisterNewAction
_processor.RegisterNewAction(new Command(), new Handler(() => akcja))
(W przypadku komendy "wyjdz", prosze uruchomic kod Enviroment.Exit(0) )
19. Przenies wypisywanie menu aplikacji do osobnej metody statycznej nazwij ja "DisplayMenu"
20. W metodzie DisplayMenu wykasuj kod odpowiedzialny za wyswietlanie nazw komend i ich opisow
21. Wykorzystaj metodę obiektu processor-a aby wyswietlic menu aplikacji
(Uwaga: bedzie potrzeba zapisac sobie obiekt procesora jako pole klasy Program, lub przekazac ten obiekt w parametrach wywolania tej funkcji)
22. Zamiast ogromnego if-a wywolaj funkcje _processor.HandleCommand(command) gdzie jako command przezkaz ciag znakow wprowadzony od uzytkownika
23. Na koniec mozesz wyniesc Autoryzacja uzytkownika do osobnej funkcji statycznej tak aby kod programu byl nieco bardziej czysty.  

#### Zadanie 12: Exceptions

1. Przejdz do projektu Library.Domain
2. Otwórz kod źródłowy klasy Book
3. Dla kazdego parametru konstruktora napisz zapezbieczenie: 
   - Dla paramterów typu string uzyj metody `string.IsNullOrWhiteSpace`
   - Dla publicationYear napisz warunek, który sprawdzi czy data publikacji jest wieksza od 1000 i mniejsza niz obecny rok (`DateTime.UtcNow.Year`)
    - Dla productsAvailable wartosc ma byc z przedzialu <0;2000)
    - Dla Price ma byc wieksze rowne 0.00m
    
    Pamietaj aby dopasowac rodzaj rzuconego wyjatka to rodzaju wykroczenia: 
    - ArgumentOutOfRangeException
    - ArgumentNullException
4. Dodaj zabezpieczenie w metodzie ChangeProductsAvailableNumber. Pamiętając ze trzeba sprawdzic stan magazynowy po operacji arytmetycznej.
5. Przejdz do klasy BookOrdered
6. Dodaj zapezpieczenia dla paramterow konstruktora aby nie mogly one przyjmowac wartosci
mniejszych od zera
7. Przejdz do projektu Library.ConsoleApp
8. W projekcie Library.ConsoleApp utworz katalog `Exceptions`
9. W katalogu Exceptions utwórz własny wyjątek `ActionNotSupportedException`, pamietaj o implementacji 3 konstruktorów
    - ActionNotSupportedException() : base()
    - ActionNotSupportedException(string message) : base(message)
    - ActionNotSupportedException(string message, Exception inner) : base(message, inner)
    
#### Zadanie 13: Atrybuty

1. Przejdz do projektu Library.ConsoleApp
2. Utwórz katalog Attributes
3. Utwórz klasę Command dziedzicącą po klasie System.Attribute
4. Klasie Command dodaj atrybut `AttributeUsage(AttributeTargets.Class)`
5. Utwórz konstruktor klasy Command, który posiada dwa paramtery
    - name: string
    - description: string
6. Stwórz dwa pola prywatne o typie string i nazwach: 
    - _name
    - _description
7. Przypisz zmienne wprowadzone przez konstruktor do pól klasy zaimplementowanych w poprzednim punkcie
8. Stwórz metodę GetName która będzie zwraca pole _name
9. Stwórz metodę GetDescription która będzie zwraca _description

#### Zadanie 14: Refleksje

1. Przejdz do projektu Library.ConsoleApp
2. Stwórz prywatne pole w klasie Processor, które będzie zawierało słownik <string, IHandler>o nazwie `actions`, od razu zainicjalizuj te pole.
3. Stwórz bez parametrowy konstruktor
4. W konstruktorze za pomocą refleksji i zapytań LINQ pobiez wszystkie typy, które implementują interfejs `IHandler`

            var type = typeof(IHandler);
            var types = AppDomain.CurrentDomain.GetAssemblies()
                .SelectMany(s => s.GetTypes())
                .Where(p => type.IsAssignableFrom(p))
                .Where(p => !p.IsInterface)
                
5. Za pomocą wyrazenia LINQ `ToDictionary` utwórz słownik
6. Jako klucz słownika zdefiniuj wyraenie lambda, które pobierze Customowy atrybut (Comand)
z danego typu, a następnie pobierze pole name
7. Jako drugie wyrazenie lambda wykorzystaj klase (IHandler)Activator.CreateInstance(Type) aby stworzyc instancje
8. Na koniec przypisz tak stworzony slownik do zmiennej _actions
9. W metodzie HandleCommand wykorzystaj blok Try, Catch, Finally aby obsłuzyc błędy podczas wykonywania komend
10. Wywal wczesniej zdefiniowany wywolanie metody Handle na obiekcie
11. Pobierz akcje do wykonania z listy akcji
12. Dodaj zabezpieczenie: Jezeli nie pobralo akcji z listy to powinno rzucic wyjatkiem ActionNotSupportedException
13. Wywolaj metodę Handle na pobranej akcji
14. W sekcji finally wywolaj metode AfterCommand
15. Stwórz katalog Handlers
16. Utworz klasy, niech kazda ponizsza klasa implementuje interfejs IHandler
    - AddBookHandler
    - ChangeStateHandler
    - ExitHandler
    - ListBookHandler
    - ListOrderHandler
    - PlaceOrderHandler
    - RemoveBookHandler
17. Przejdz do projektu Library.Persistence
18. Stwórz klasyczną klasę Repositories
19. W klasie repositories dodaj dwa statyczne pola 

    public static readonly BooksRepository BooksRepository = new BooksRepository();
    public static readonly OrderRepository OrderRepository = new OrderRepository();
    
20. W kazdej klasie *Handler Przekopiuj odpowiednia logikę do metody Handle: 
    np. Dla klasy AddBookHandler trzeba przekopiowac logikę z klasy BooksService z metody AddBook
21. Dla handler-a `ExitHandler` ustaw implementacje metody Handle jako `Enviroment.Exit(0)`
22. Po dodaniu logiki do kazdego handler-a mozesz skasowac klasy BooksService i OrdersService
23. Mozesz skasowac ICommand i Command
24. Przejdz do pliku Program.cs
25. Skasuj odwolania do klas Command, BooksService i OrdersService
26. Skasuj wszystkie wywolania metody RegisterNewAction na obiekcie procesor
27. W pliku procesor mozesz skasowac metode RegisterNewAction
28. w pliku Processor.cs w metodzie GetAllCommandsWithDescription do wyrazenie LINQ, które z wartosci(_actions.Values) słownika pobierze typy.
29. Następnie za pomocą refleksji otrzyma informacje z Atrybutu Command (name, description)
30. Za pomocą metody `.Select((x) => $"{x.Key} - {x.Value}");` wygeneruje tablice podpisów w menu
31. za pomoca metody `string.join("\n", methods.ToArray());` zwróci menu aplikacji
    