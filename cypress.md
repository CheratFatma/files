*****************************************************************************************************************************************
                                                            Projet POM
*****************************************************************************************************************************************
# INSTALLATIONS 
* npm init -y
* npm install cypress@14 --save-dev (version 14 imposée car dernière version 15 a des soucis )
* npx cypress --version 
* npm install --save-dev @cypress/grep@5.0.0 (pour utiliser les tags)
* npm install csvtojson --save-dev (installer la bibliotheque csvtojson pour les jeux de donnees)
* npm install mochawesome mochawesome-merge mochawesome-report-generator --save-dev (pour utiliser mockawesome)
* npm install --save-dev @shelex/cypress-allure-plugin allure-commandline (pour utiliser Allure)
* npm install --save-dev @faker-js/faker (pour generer des donnees fake)

* lancer des test la premiere fois
    * npx cypress open
    * choisir e2e testing daans l'interface 
    * cliquer continuer 
    * choisir Chrome
    * cliquer Start E2E Testing in Chrome
    * cliquer create new spec
    * cliquer create spec 
    * cliquer Okay, run the spec
    * remarque : fichier spec.cy.js ==> tu peux le supprimer et c'est la que tu cree ton test à toi ==> tu peux le nommer login.cy.js

# CONFIG RAPPORT MOCHAWESOME
* dans cypress.config.js
    * ajouter 
        // reporter Mochawesome
        reporter: "mochawesome",
        reporterOptions: {
            reportDir: "cypress/reports/mochawesome",
            overwrite: false,
            html: false,
            json: true
        },
    * comme ceci : (JUSTE POUR SAVOIR L'EMPLACEMENT)
        module.exports = defineConfig({
            // reporter Mochawesome
            reporter: "mochawesome",
            reporterOptions: {
                reportDir: "cypress/reports/mochawesome",
                overwrite: false,
                html: false,
                json: true
            },

            e2e: {
                setupNodeEvents(on, config) {
                // implement node event listeners here
                },
            },
        });
* dans package.json
    * supprimer 
        "scripts": {
            "test": "echo \"Error: no test specified\" && exit 1"
        },
    * remplacer avec : 
        "scripts": {
            "cy:run": "cypress run",
            "cy:open": "cypress open",
            "report:merge": "mochawesome-merge cypress/reports/mochawesome/*.json > cypress/reports/report.json",
            "report:generate": "marge cypress/reports/report.json -f report -o cypress/reports"
        }, 

# CONFIG RAPPORT ALLURE
* dans cypress.config.js
    * ajouter 
        const allureWriter = require('@shelex/cypress-allure-plugin/writer'); 
 
        // plugin allure
        env: {
            allure: true,
            allureResultsPath: 'allure-results',
            allureReuseAfterSpec: true,
        }, 

        // plugin allure
        allureWriter(on, config);
        return config 
    * comme ceci : (JUSTE POUR SAVOIR L'EMPLACEMENT)
        const { defineConfig } = require("cypress");
        const allureWriter = require('@shelex/cypress-allure-plugin/writer'); 

        module.exports = defineConfig({
            e2e: {
                // plugin allure
                env: {
                    allure: true,
                    allureResultsPath: 'allure-results',
                    allureReuseAfterSpec: true,
                }, 

                setupNodeEvents(on, config) {
                    // plugin allure
                    allureWriter(on, config);
                    return config 
                },
            },
        });
* dans dossier support, dans fichier e2e.js
    * ajouter 
        import '@shelex/cypress-allure-plugin'; 


# COMMANDES
* lancer des test en mode headless
    * npx cypress run
* lancer des test en mode headfull 
    * npx cypress open
* lancer avec les tags
    * npx cypress run --env grepTags=@smoke 
* exécuter avec environnement et viser un seul test
    * npx cypress run --spec "cypress/e2e/loginenv.cy.js" --env var=recette 
* generer rapport Mochawesome
    * npx cypress run 
    * npm run report:merge
    * npm run report:generate
* generer rapport Allure
    * npx cypress run 
    * npx allure generate allure-results --clean -o allure-report 
    * npx allure open allure-report 

*****************************************************************************************************************************************
                                                            Projet BDD
*****************************************************************************************************************************************
# INSTALLATIONS 
* les memes que ceux de pom + ajouter cucumber
    * npm install --save-dev cypress cucumber cypress-cucumber-preprocessor 

# CONFIG CUCUMBER
* creer profile nommer comme on veut (ex:cypress)
* se mettre sur cypress
* activer auto-save
* aller dans extention et installer cucumber 
    * aller dans parametre de cucumber 
        * dans la partie cucumber.glue ajouter (on met les deux)
            * "cypress/e2e/**/*.feature", (si projet déployé et le dossier e2e n’est pas supprimé )
            * "cypress/steps/**/*.js", (sinon) --- comme notre cas 

* dans cypress.config.js
    * ajouter 
        const cucumber = require("cypress-cucumber-preprocessor").default; //a ajouter pour le preprocessor

        specPattern: "**/*.feature", // a ajouter pour le preprocessor 

        on("file:preprocessor", cucumber()); // ajouter pour le preprocessor
    * comme ceci : 
        const { defineConfig } = require("cypress");
        const cucumber = require("cypress-cucumber-preprocessor").default; //a ajouter pour le preprocessor

        module.exports = defineConfig({
            
            e2e: {
                specPattern: "**/*.feature", // a ajouter pour le preprocessor

                setupNodeEvents(on, config) {
                    on("file:preprocessor", cucumber()); // ajouter pour le preprocessor
                },
            },
        });

* dans package.json
    * ajouter a la fin
        "cypress-cucumber-preprocessor": {
            "nonGlobalStepDefinitions": false,
            "step_definitions": "cypress/steps"
        } 

# COMMANDES
* lancer les test
    * npx cypress open
* lancer un fichier spec:
    * npx cypress run --spec "cypress/features/login.feature"

*****************************************************************************************************************************************
                                                    Squelette Fichier Test .cy.js
*****************************************************************************************************************************************
//directive de cypress de reference
///<reference types="cypress"/>
describe("",()=>{
     beforeEach('setup',()=>{
        cy.visit('https://www.saucedemo.com/');
    })

    afterEach('teardown',function(){
        if(this.currentTest.state === "failed"){
            cy.screenshot(`failure-${this.currentTest.title}`)
        }
        cy.clearAllCookies();
        cy.clearAllLocalStorage();
        sessionStorage.clear();
    })

    it("login valide", ()=>{

    })

    it("login invalide", ()=>{
        
    })
})

* si j'utilise this je suis obliger d'utiliser function() au lieu de la fonction fléchée ()=>{}
* pour les tags, on les ajoute au niveau des it
    * it("login valide",{tags:"@loginValide"}, ()=>{ }) ------ un seul tag
    * it("login invalide", {tags:["@loginInvalide","@TNR"]}, ()=>{ }) ---- plusieurs tags

*****************************************************************************************************************************************
                                                Squelette Fichier Page .cy.js
*****************************************************************************************************************************************
///<reference types="cypress" />
class Loginpage {
    //les locators
    elements = {
        unsername :() => cy.get('#user-name'),
        click_button:() => cy.get('#login-button'),
        error_message:() => cy.get('[data-test="error"]')
    }

    //les actions
    saisirusername(u){
        this.elements.unsername().type(u);
    }
    click_button_connexion(){
        this.elements.click_button().click();
    }
    error(){
       return this.elements.error_message();
    }
}
export default new Loginpage();
*****************************************************************************************************************************************
                                                        Exercices Réalisés 
*****************************************************************************************************************************************
# login.cy.js
*****************************************************************************************************************************************
# loginenv.cy.js
beforeEach("setup",()=>{
        let environment = Cypress.env("var")
        let url 

        switch (environment) {
            case ("integration"):
                url = "https://www.saucedemo.com/"
                break;
            case ("recette"):
                url = "https://www.saucedemo.com/recette"
                break;
            case ("preprod"):
                url = "https://www.saucedemo.com/preprod"
                break;
            default:
                url = "https://www.saucedemo.com/"
        }
        cy.visit(url);
})
*****************************************************************************************************************************************
# loginjson.cy.js
    * Dans le dossier fixtures
        * creer jdd.json 
    * utiliser 
        cy.fixture("jdd.json").then((data)=>{
            data.forEach((user) => {
                // A FAIRE
            })
        })
it("login with jdd json",()=>{
    cy.fixture("jdd.json").then((data)=>{
        data.forEach((user) => {
            cy.visit("https://www.saucedemo.com/")
            cy.get('#user-name').type(user.username);
            cy.get('#password').type(user.password);
            cy.get('#login-button').click();
            if(user.result === "success") {
                cy.url().should('include', '/inventory.html');
            }
            else {
                cy.get('[data-test="error"]').should('be.visible');        
            }    
        });   
    })   
})
*****************************************************************************************************************************************
# loginjddwrap.cy.js
    * Dans le dossier fixtures
        * creer jdd.json 
    * utiliser 
        cy.fixture("jdd.json").then((data)=>{
            cy.wrap(data).each((user)=>{
                // A FAIRE
            });
        })
*****************************************************************************************************************************************
# logincsv.cy.js
    * Dans le dossier fixtures
        * creer jdd.csv 
    * Importer 
        * import csv from "csvtojson"
    * utiliser 
        cy.readFile("cypress/fixtures/jdd.csv").then((data)=>{
            csv().fromString(data).then((users)=>{
                users.forEach((user) => {
                    // A FAIRE
                })
            })
        })
*****************************************************************************************************************************************
# logincsv2.cy.js
    * Dans le dossier fixtures
        * creer jdd.csv 
    * Importer 
        * import Papa from "papaparse"
    * utiliser 
        cy.readFile("cypress/fixtures/jdd.csv").then((data)=>{
            const res = Papa.parse(data, {
                header: true,
                skipEmptyLines: true
            })
            cy.wrap(res.data).each((user) => {
                // A FAIRE
            })
        })
*****************************************************************************************************************************************
# login_pom.cy.js
* creer un dossier pages dans le dossier cypress
    * creer fichier login.page.js avec les locators et les actions comme montré en haut
    
* Dans le fichier de test login.cy.js importer 
    * import loginPage from "../pages/login.page.js";
* pas besoin de l'initialiser 
    * faire directement loginPage.saisirUsername('standard_user')
*****************************************************************************************************************************************
# product.cy.js
exemple de parcours produit avec l'approche POM juste pour s'appliquer

*****************************************************************************************************************************************
                                                        Exercices Réalisés BDD
*****************************************************************************************************************************************
* pour tous les exo bdd , dans le dossier cypress
    * supprimer le dossier e2e
    * creer dossier features
    * creer dossier steps
    * creer dossier pages
*****************************************************************************************************************************************
# login.feature

Feature: fonctionnalite d'authentification
    Background:
        Given visiter le site sauce demo "https://www.saucedemo.com/"
    
    @valide
    Scenario: login valide
        When je saisi username "standard_user"
        And je saisi password "secret_sauce"
        And je clique sur le bouton login
        Then verifier si on est sur le bon url
    
    @invalide
    Scenario: login invalide
        When je saisi username "standard_user_fake"
        And je saisi password "secret_sauce_fake"
        And je clique sur le bouton login 
        Then verifier le message erreur

* petite astuce pour faire le lien entre la Feature et la step correspondante
    * Dans feature mettre Given lamia
    * Dans step mettre Given("lamia",()=>{ })
    * pour le reste on utilise la loupe
* NE PAS OUBLIER DE LA SUPPRIMER DES DEUX COTES UNE FOIS LE LIEN FAIT

# login.step.js

Given('visiter le site sauce demo {string}',(url) => {
     cy.visit(url)
})

Given('je saisi username {string}',(username) => {
    cy.get('#user-name').type(username);  
})

When('je clique sur le bouton login', () => {
    cy.get('#login-button').click();
})

Then('verifier si on est sur le bon url', () => {
    cy.url().should('include', '/inventory.html');
})

Then('verifier le message erreur', () => {
    cy.get('[data-test="error"]').should('be.visible');        
})
*****************************************************************************************************************************************
# loginjdd.feature
Feature: fonctionnalite d'authentification - JDD
    Background:
        Given visiter le site sauce-demo "https://www.saucedemo.com/"
    
    @jdd
    Scenario Outline: login valide JDD
        When je saisi usernamee "<username>"
        And je saisi passwordd "<password>"
        And je clique sur le bouton loginn
        Then verifier url ou error "<res>"
    
    Examples:
        |username|password|res|
        |standard_user|secret_sauce|ok|
        |standard_user_fake|secret_sauce_fake|ko|

# loginjdd.step.js
Given('visiter le site sauce-demo {string}', (url) => {
         cy.visit(url)
})

When('je saisi usernamee {string}', (username) => {
    cy.get('#user-name').type(username);  
})

When('je saisi passwordd {string}', (password) => {
    cy.get('#password').type(password);
})

When('je clique sur le bouton loginn', () => {
    cy.get('#login-button').click();
})

Then('verifier url ou error {string}', (s) => {
    if (s == "ok"){
        cy.url().should('include', '/inventory.html');
    }
    else {
        cy.get('[data-test="error"]').should('be.visible');        
    }
})
*****************************************************************************************************************************************
# loginFixtures.feature
Feature: fonctionnalite d'authentification - JDD
    Background:
        Given visite le site sauce-demo "https://www.saucedemo.com/"
    
    @Smoke @Valide
    Scenario Outline: login valide with Fixtures
        When je saisie la cle "<cle>" 
        And je clique sur le buton login
        Then verifier url or error "<res>"
    
    Examples:
        |cle|res|
        |valid_user|ok|
        |invalid_user|ko|
        |invalid_password|ko|
        |user_without_username|ko|
        |user_without_password|ko|
        |user_empty|ko|

# jdd.json (DANS DOSSIER FIXTURES)
{
  "valid_user" : {
    "username":"standard_user",
    "password":"secret_sauce"
  }, 
  "invalid_user" : {
    "username":"standard_user_fake",
    "password":"secret_sauce"
  },
   "invalid_password" : {
    "username":"standard_user",
    "password":"secret_sauce_fake"
  },
   "user_without_username" : {
    "username":" ",
    "password":"secret_sauce"
  },  
  "user_without_password" : {
    "username":"standard_user",
    "password":" "
  },
  "user_empty" : {
    "username":" ",
    "password":" "
  }
}

# loginFixtures.step.js
import LoginPage from "../pages/login.page"


Given('visite le site sauce-demo {string}', (s) => {
    cy.visit(s)
})

When('je saisie la cle {string}', (cle) => {
    cy.fixture("jdd.json").then((users) => {
        const data = users[cle]
        //cy.get("#user-name").type(data.username)
        //cy.get("#password").type(data.password)
        LoginPage.saisir_username(data.username)
        LoginPage.saisir_password(data.password)
    })
})

When('je clique sur le buton login', () => {
    //cy.get('#login-button').click();
    LoginPage.click_cnx()

})

Then('verifier url or error {string}', (s) => {
    if (s == "ok"){
        cy.url().should('include', '/inventory.html');
    }
    else {
        cy.get('[data-test="error"]').should('be.visible');        
    }
})
*****************************************************************************************************************************************
