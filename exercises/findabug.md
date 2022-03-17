## Find a bug

Clone the [Simba Organizer repository](https://github.com/barais/doodlestudent/) and follow the instructions to run the application on your machine.

Find a bug in the application. 

With the help of Selenium and the Page Object Model desing pattern write a simple test that fails for this bug.

Optionally make a pull request to the project.

Include in this document the code of the test and, if you did it, the link to the pull request.

## Answer

Le bug trouvé est obtenu assez facilement. Lorsqu'on lance l'application et qu'on souhaite créer un profil, on se retrouve sur un premier onglet qui concerne les informations pour le rendez-vous. Normalement, pour accéder à l'onglet suivant, trois champs doivent être remplis. Si on ne remplit aucun de ces champs et qu'on clique sur le bouton Next, des messages d'erreurs apparaissent sous les inputs pour demander à l'utilisateur de les renseigner. En revanche si on clique directement sur le lien '2 Choix de la date' ou le lien '3 Résumé', aucune vérification n'est effectuée et on accède directement à un des onglets suivants. C'est donc bien un bug car on ne devrait pas pouvoir accéder aux autres onglets tant que les champs présents dans le premier onglet ne sont pas renseignés. Le cas de test ci-dessous permet de détecter ce bug.

```java
import io.github.bonigarcia.wdm.WebDriverManager;
import org.junit.jupiter.api.BeforeAll;
import org.junit.jupiter.api.Test;
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;

import java.util.HashSet;
import java.util.List;
import java.util.Set;

import static org.junit.jupiter.api.Assertions.assertFalse;
import static org.junit.jupiter.api.Assertions.fail;

public class TestSORPage {

    private static WebDriver webDriver;
    private static Set<String> listPages;
    private static String lastUrl;
    private static String newUrl;

    @BeforeAll
    public static void setUp(){

        listPages = new HashSet<>();

        lastUrl = "";

        newUrl = "";

        System.setProperty("webdriver.chrome.driver", "C:\\Users\\Erwann\\chromedriver_win32\\chromedriver.exe");

        WebDriverManager.chromedriver().setup();

        webDriver = new ChromeDriver();

        // Setting the browser size
        webDriver.manage().window().setSize(new Dimension(1024, 768));
    }

    @Test
    public void testClickButton3WithEmptyFields() {
        // Go to wikipedia
        newUrl = "http://localhost:4200/create";
        webDriver.navigate().to(newUrl);

        try {
            List<WebElement> allLinks = webDriver.findElements(By.tagName("a"));
            WebElement button3 = null;
            for (WebElement element : allLinks) {
                if (element.getText().trim().contains("3")) {
                    button3 = element;
                    element.click();
                    break;
                }
            }
            if (button3 != null) {
                SummaryPage page = new SummaryPage(webDriver);
                assertFalse(page.title().trim().isEmpty());
                assertFalse(page.place().trim().isEmpty());
                assertFalse(page.description().trim().isEmpty());
            } else {
                fail("No button '3' found.");
            }
        } catch (ElementNotInteractableException e) {
            fail("Impossible to click on button '3'.");
        }

        webDriver.quit();
    }
}
```