ETAPE 1 : créer projet MVN

* view => commande palette => remplir "project" => choisir java: create java project => puis mvn => choisir arctype-quickstart => version 1.4 => com.example => demo =< faire entrer (2fois) sur terminal jusqu'à build success
* cd demo
* mvn clean compile : a utiliser si j'effectue des modif dans pom.xml


ETAPE 2 : tests unitaires

* cd demo
* mvn clean compile : a utiliser si j'effectue des modif dans pom.xml
* pour les testeurs (dossier test), pour les dev (dossier main)
* on est obliger de nommer le fichier de Test avec a la fin "Test" lors du nommage du fichier
* mvn test OU mvn clean test : pour lancer tout les tests
* mvn test -Dgroups=smoke : pour lancer avec tag
* mvn test -Dtest=fichierTest : pour lancer le fichier de test
* mvn surefire-report:report : pour lancer le rapport


ETAPE 3 : automatisation

* ajouter la dependance selenium dans pom.xml : view => command palette => maven add dependency => chercher java-selenium => cliquer sur entrer
* driver est null veut dire driver pas initialiser


ETAPE 4 : Jeux de données CSV
* @ParameterizedTest 
* @CsvFileSource (resources = "/jdd2.csv", numLinesToSkip = 1)


ETAPE 5 : Cross-Browsing

* creer une instense du navigateur (creer donc une variable system) :  String nav = System.getProperty("browser","chrome");
* mvn test -Dtest=fichier -Dbrowser=chrome
* mvn test -Dtest=fichier -Dbrowser=firefox
* mvn test -Dtest=crossTest -Dbrowser=safari (dans ce cas le browser par defaut va se lancer car il n'est pas declarer)


ETAPE 6 : Execution Paralel / sequentiel

* ajouter une configuration parameters dans le pom.xml
* parallel : junit.jupiter.execution.parallel.enabled = true
* sequentiel : junit.jupiter.execution.parallel.enabled = false
* lancer les test : mvn test
* resultats : 20.409 s : sequentiel / 19.500 s : parallel


ETAPE 7 : Await

* implicit wait : driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
* explicit wait : 
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(15));
    wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("login-button"))).click();


ETAPE 8 : docker-compose / grid selenium

steps:
* allumer docker desktop 
* deployer le fichier docker-compose.yml dans docker desktop

les scripts vont envoyer des requete à hub et c'est remotewebdriver qui les envoi et c'est lui qui pilote distanciellement le navigateur . puis le hub va les etuider et decider vers quel node les envoyer (node1 , node2, node3)

442 EST CE QUE TU ES DISPO POIR EXECUTER SUR CROME
443 OUI DISPO 
444 PUIS S'EXECUTE ICI

au moment ou vous solliciter le navigateur distanciel , on l'aura avec (http://localhost:4444/ui/)


* docker-compose.yml : fichier contient des services (des navigateurs) , toute la configuration et la mettre dans docker desktop
* lancer l'installation des differentes images : docker-compose up -d
* http://localhost:4444/ui/ : lien
* 127.1.0.1:4444/ui/ : lien
* grid : 
     case "grid":
        URL gridUrl = null;
        try {
            gridUrl = new URL("http://127.1.0.1:4444");
        } catch (MalformedURLException e) {
                    e.printStackTrace();
        }
            ChromeOptions co = new ChromeOptions();
            FirefoxOptions fo = new FirefoxOptions();
            EdgeOptions eo = new EdgeOptions();
                driver = new RemoteWebDriver(gridUrl, co);
                break;


ETAPE 9 : POM (Page Object Model)

* Syntaxe 
WebDriver driver;
By username = By.id("user-name");

* logique 
public void fillUsername(String u){
    driver.FindElement(username).sendKeys(u)
}

* constructeur
public loginPage(WebDriver driver){
    this.driver = driver;
}


ETAPE 10 : Page Factory
* Syntaxe 
@FindBy(id="user-name") private WebElement username;

* logique (se resume au lazy loading)
public void fillUsername(String u){
    username.sendKeys(u)
}

* difference avec POM : 
dans Page Factory : on peut dire que la recherche de l'element se fait dans la partie de declaration lors du FindBy car selenium le fait donc pas besoin de FindElement, elle se fait en arriere plan

donc au moment de l'action (lors de l'appelle à la methode, selenium fait la recherche directement et donc pas besoin du FindElement, on lui donne juste le webElement)

* constructeur
public loginPage(WebDriver driver){
    PageFactory.initElements(driver, this);
}


ETAPE 11 : Rapport Allure

* mvn clean test
* mvn allure:serve


ETAPE 12 : BDD

* view => commande palette => remplir "project" => choisir java: create java project => puis mvn => choisir cucmber-archtype => version 1.7.11 => com.example => demo =< faire entrer (2fois) sur terminal jusqu'à build success
* cd demo
* mvn clean compile : a utiliser si j'effectue des modif dans pom.xml


* configuration cucmber 
* ajouter l'extention cucumber
* ajouter dans cucmber.glue : "**/steps/*.java",

* ajouter une dependance
* view => command palette => maven add dependency => chercher selenium-java => cliquer sur entrer

ETAPE 13 : Rapport BDD
* dans fichier RunCucumberTest
* ajouter html et json report
* @ConfigurationParameter(key = PLUGIN_PROPERTY_NAME, value = "pretty, html:target/html_report.html, json:target/junit_report.xml")


ETAPE 14 : exo demo web shop
CLICK REGISTER 
REMPLIR LE FORMULAIRE 
click register 
verifier nouveau url 
cliquer sur continue
verifier nouveau url
click log out
click log in
remplir formulaire 
cliquer remember me
cliquer login 
verifier soit logout afficher ou email afficher (au choix)
cliquer add to cart 
verifier nouveau url
saisir name 
saisir email 
cliquer add to cart 
verifier l'affichage 
sur cet afficher cliquer sur shopping cart 
click agree 
click checkout
verifier url 
remplir le formulaire (meme les cases non obligatoire)
cliquer continue
verifier url :::::::
choisir credit card 
click continue 
saisir name
saisir card number 
saisir card code
cliquer continue 
verifier url 
cliquer confirm 
verifier url 
verifier msg succes order

pf : register 
reste : pom


rapport - surefire - allure
* mvn surefire-report:report

* mvn clean test
* mvn allure:serve




************************************************************************************************************************************
                                                    ETAPES GENERALES - POM - PF
************************************************************************************************************************************

* view => commande palette => remplir "project" => choisir java: create java project => puis mvn => choisir arctype-quickstart => version 1.4 => com.example => demo =< faire entrer (2fois) sur terminal jusqu'à build success


* ajouter la dependance selenium dans pom.xml : view => command palette => maven add dependency => chercher java-selenium => cliquer sur entrer

commande CMD : 
* mvn clean compile (se trouver dans artifactId)
* mvn test
* mvn test -Dtest=nomTest
* mvn test -Dgroups=tag
* mvn test -Dtest=fichier -Dbrowser=chrome : propriete systeme
* mvn surefire-report:report : pour lancer le rapport
* mvn allure:serve (faut faire avant mvn clean test)

coté code:
* initialiser Webdriver driver;
* driver = new ChromeDriver();
* @Test public void loginValid(){}
* @Parametrized Test 
@CsvFileSource (resources = "/jdd2.csv", numLinesToSkip = 1) --- dossier ressource au meme stade que java dans test
public void loginJDD(parametres_jdd){}

* driver.findElement(By.id ou By.className ou By.cssSelector OU By.xpath RELATIF MAIS ABSOLU INTERDIT OU DOM)
* click, sendKeys, isDesplayed, getText, .... 

DESIGN PATERN DE CONCEPTION
* POM
By username = By.id("user-name);
public loginPage(WebDriver driver){
    this.driver = driver;
}

public void fillUsername(String u){
    driver.findElement(username).sendKeys(u);
}

* PF 
@FindBy(id="user-name") private WebElement username;
public loginPage(WebDriver driver){
    PageFactory.initElements(driver, this);
}
public void fillUsername(String u){
    username.sendKeys(u);
}


* TEMPS D'ATTENTE
* implicit wait : driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(10));
* explicit wait : (generalement pour les boutons ou url redirection)
    WebDriverWait wait = new WebDriverWait(driver, Duration.ofSeconds(15));
    wait.until(ExpectedConditions.visibilityOfElementLocated(By.id("login-button"))).click();

* SELENIUM GRID

* dans les environnement de test pas de captcha c'est en prod qu'on les trouve
* local === envoie des requetes ==== virtuel (dockerdesktop) === fichier (docker-compose.yml) ==== ce fichier a besoin d'images === ces images sont présentes dans docker hub


* A EVITER 

* eviter thread.sleep()
* eviter try catch ===== si c'est necessaire oui mais si on peut eviter on evite
* assurer l'independance
* separation logique metier / logique test
* separation jdd
* ajout des assertions
* eviter repetition (hooks ou fonctionalités réutilisables)

**************************************************************************************************************************************
                                                        ETAPES GENERALES - BDD 
**************************************************************************************************************************************
* def : approche de test et de developpement , se repose sur un langage qui est descriptif simple detaillé qui est le langage gherkin, basé sur Given (pointer le demarrage), When (moment de l'action) , And (le reste des action) ,Then (pointer la fin - assertions)

* interet : faciliter la collaboration au sein de toute l'equipe (technique et non-technique)

* automatisation : creer une .feature pour decrire le comportement 
    * cas de test automatique : traduire .feature en .step grace a cucumber qui est une implementation de la bibliotheque de bdd
    * cas de test fonctionnel : implementer le cas de test xray/squash

******
STEPS: 
******
* ajouter le profil
* creer le projet 
    * view => commande palette => remplir "project" => choisir java: create java project => puis mvn => choisir cucmber-archtype => version 1.7.11 => com.example => demo =< faire entrer (2fois) sur terminal jusqu'à build success

* ajouter configuration cucmber 
* ajouter l'extention cucumber
* ajouter dans cucmber.glue : "**/steps/*.java",


* ajouter une dependance
* view => command palette => maven add dependency => chercher selenium-java => cliquer sur entrer

* cd demo
* mvn clean compile : a utiliser si j'effectue des modif dans pom.xml

* organisation de l'arboresences : 
src/test/resources/com/example : creer fichier .feature
src/java/com/example : creer dossier steps et dedans les fichier stepDefinitions

* commandes importantes pour l'execution des tests
mvn test : lancer tous les test 
mvn test -D cucumber.Features=".Feature"
mvn test -Dgroups=tag
mvn test -D cucmber.filter.tags="@tag"

* generation des rapports : 
* dans fichier RunCucumberTest
* ajouter html et json report
* @ConfigurationParameter(key = PLUGIN_PROPERTY_NAME, value = "pretty, html:target/html_report.html, json:target/junit_report.xml")

* architecture (.Feature)
Feature:
    Background:
        Given
        When
        Then
    @tag
    Scenario: description_scenario
        Given
        When
        Then
    Scenario Outline:

    Examples:

* architecture (.step)
@Given("test)
public void test(){}


cliquer sur register 
remplir le formulaire et valider 
verifier welcome username
cliquer log out 
verifier url ou verifier le text customer login
remplir formulaire login
cliquer login 
verifier accounts overview affiché



 



