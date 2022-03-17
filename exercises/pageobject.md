## Page Object Model

The image below shows the poll page of the [Simba Organizer](https://github.com/barais/doodlestudent/) application discussed in classes.

![Simba Organizer Poll page](simba-poll-page.png)

Write in this document the interface of a page object class for this page.

## Answer

```java
import java.util.List;
import java.util.Map;

public interface PollPage {

    // Menu
    InformationPage createNew();

    String urlToShare();

    String chatRoomURL();

    String padURL();

    // Elements non modifiables par l'utilisateur
    String title();

    String place();

    boolean hasMeal();

    List<OptionPanel> options();

    Map<String,String> commentaires();

    Map<String,List<Boolean>> listeVoeux();

    // Elements modifiables
    String nomPrenom();

    PollPage typeNomPrenom(String value);

    String mail();

    PollPage typeMail(String value);

    boolean hasICS();

    PollPage setHasICS(boolean value);

    boolean hasPrefsAliments();

    PollPage setHasPrefsAliments(boolean value);

    boolean vueTableau();

    PollPage setVueTableau(boolean value);

    boolean vueCalendrier();

    PollPage setVueCalendrier(boolean value);

    List<Boolean> voeux();

    PollPage setVoeux(List<Boolean> values);

    String auteurCommentaire();

    PollPage typeAuteurCommentaire(String value);

    String commentaire();

    PollPage typeCommentaire(String value);

    // Boutons et erreurs
    List<String> errors();

    PollPage soumettreVoeux();

    RecapitulatifPage soumettreVoeuxExpectingErrors();

    PollPage soumettreCommentaire();

    PollPage soumettreCommentaireExpectingErrors();
}
```


