Kluczowy element architektury aplikacji opartej na Springu.
Serwisy są używane do implementacji logiki biznesowej oddzielając ją od warstwy kontrolerów oraz warstwy repozytoriów.

- Adnotacja `@Service`:
	- Serwisy są oznaczane adnotacją `@Service`. Dzięki temu Spring automatycznie wykrywa i rejestruje klasy serwisów jako komponenty w kontekście aplikacji
	- Przykład:
```java
import org.springframework.stereotype.Service;

@Service
public class UserService {
    // logika biznesowa
}
```

- Iniekcja zależności (Dependency Injection):
	- Serwisy mogą być automatycznie wstrzykiwane do innych komponentów, takich jak kontrolery czy inne serwisy, poprzez adnotację `@Autowired`.
	- Przykład:
```java
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;

@Controller
public class UserController {

    @Autowired
    private UserService userService;

    // logika kontrolera
}

```

- Separacja problemów (Separation of Concerns)
	- Serwisy pomagają w rozdzieleniu odpowiedzialności w aplikacji, co sprzyja organizacji kodu i łatwiejszemu testowaniu. Logika biznesowa jest oddzielona od logiki prezentacji i logiki dostępu do danych.

- Transakcje
	- Serwisy zarządzają transakcjami w aplikacjach korzystających z baz danych. Można użyć adnotacji `@Transactional` na poziomie klasy lub metody, aby zapewnić, że operacje są wykonywane w sposób atomowy.
	- Istnieją różne atrybuty adnotacji, takie jak: 
		- **propagation**: zarządzanie transakcją, gdy metoda jest wywoływana w ramach istniejącej już transakcji
		- **isolation**: widoczność zmian dokonanych w ramach transakcji dla innych transakcji.
		- **timeout**: maksymalny czas w sekundach przez jaki transakcja moze trwać przed zakończeniem
		- **readOnly**: wskazuje, że transakcja jest tylko do odczytu
		- **norollback/rollbackFor**: nie/wycofuje transakcje w przypadku wystąpienia określonego wyjątku
	- Przykład:
```java
import org.springframework.transaction.annotation.Transactional;

@Service
public class UserService {

    @Transactional
    public void registerUser(User user) {
        // logika rejestracji użytkownika
    }
}
```

- Testowanie
	- Serwisy mogą być łatwo testowane za pomocą testów jednostkowych i integracyjnych.
	- Dzięki oddzieleniu logiki biznesowej od innych warstw, serwisy można testować niezależnie.