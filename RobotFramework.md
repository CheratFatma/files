# PREREQUIS
* creer le profil RobotFramework dans VSCodeT
* installer les extentions 
    * RobotCode - Robot Framework Support
    * Robocorp Code
    * Rainbow CSV
    * open in browser
    * celles de python 

# INSTALLATIONS 
* verifier version de python 
    * python --version 
* creation de l'environnement virtuel 
    * python -m venv .venv 
* activer l'environnement virtuel 
    * .venv\Scripts\activate.bat (sur terminal en mode cmd)
    * .venv\Scripts\Activate.ps1 (sur terminal en mode PowerShell)
* creer un fichier .robot 
    * s'il ne detecte pas quand on met ***
    * remarque : si j'arrive pas a insatller 
        * python -m ensurepip --upgrade
        * python -m pip install robotframework ==== si pip install robotframework ne fonctionne pas
* pour les jeux de donnée, installer 
    * pip install robotframework-datadriver
* prob de synchronisation
    * implicit wait : attente globale
        * Set Selenium Implicit Wait    10s
    * explicit wait : attente spécifique / conditionnée
        * Wait Until Element Is Visible    id:login-button    10s   
    
pip install --upgrade robotframework-seleniumlibrary
 

# COMMANDES
* executer un test spécifique
    * robot tests/login.robot
* executer avec les tags
    * robot --include e2e tests (smoke c'est le nom du tag)
    * robot -i e2e .
* executer avec un browser specifique 
    * robot -v BROWSER:firefox tests/loginPage.robot
* lancer les test sur tout les browser en parallel
    * creer les 3 fichiers "chrome.txt firefox.txt edge.txt"
        * mettre dans chacun : --variable BROWSER:chrome (changer avec firefox ou edge)
    * installer pabot
        * python -m pip install robotframework-pabot
    * executer en sequentiel 
        * pabot --argumentfile1 ./chrome.txt --argumentfile2 ./firefox.txt --argumentfile3 ./edge.txt tests/loginPage.robot
    * executer en parallel 
        * pabot --processes 3 --argumentfile1 ./chrome.txt --argumentfile2 ./firefox.txt --argumentfile3 ./edge.txt tests/loginPage.robot
    * lancer le rapport allure 
        * python -m pip install allure-robotframework
        * npm install -g allure-commandline
        * robot --listener allure_robotframework:allure-results tests/
        * allure serve allure-results


s'inscrire 
verifier le msg d'insciprion s'affiche