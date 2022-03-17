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
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import org.openqa.selenium.*;
import org.openqa.selenium.chrome.ChromeDriver;

import static org.junit.jupiter.api.Assertions.*;

public class TestSORPage {

    private static WebDriver webDriver;

    @BeforeEach
    public void setUp(){

        System.setProperty("webdriver.chrome.driver", "C:\\Users\\Erwann\\chromedriver_win32\\chromedriver.exe");

        WebDriverManager.chromedriver().setup();

        webDriver = new ChromeDriver();

        // Setting the browser size
        webDriver.manage().window().setSize(new Dimension(1024, 768));
    }

    @Test
    public void testClickButton3WithEmptyFields() {
        webDriver.navigate().to("http://localhost:4200/create");

        try {
            WebElement button3 = webDriver.findElement(By.xpath("//span[contains(text(), '3')]"));
            button3.click();

            SummaryPage page = new SummaryPage(webDriver);
            WebElement span3 = webDriver.findElement(By.xpath("//span[contains(text(), '3')]"));
            assertEquals("rgba(0, 123, 255, 1)", span3.getCssValue("background-color"));
            assertFalse(page.title().trim().isEmpty());
            assertFalse(page.place().trim().isEmpty());
            assertFalse(page.description().trim().isEmpty());
        } catch (ElementNotInteractableException e) {
            fail("Impossible to click on span '3'.");
        }

        webDriver.quit();
    }
}
```