# HSM-branched

This is based on the HexagonalSpring-Modular. It is ment to have a structure for a development team to build upon.<br><br>

The core branches are the master/main, domain, queries, modular_monolith.<br>
Those along with the "implementation module" branches and the "service_to_be_deployed" must never be deleted and be protected.
<br><br>

![branches drawio](https://user-images.githubusercontent.com/82568995/233063441-c141760a-d4a0-48a2-ac36-8c3f7eaacd98.png)

Master/main:<br>
Contains the Spring scanner which rarely changes. This branch is the cross point to create(for the latest commit in the branch)<br>
new "Interface modules" and "Services to deploy (ex. account_movie_service)" branches.<br>

<br>
Domain:<br>
It is an "interface module". This is cross point to create<br>
new "domain_imp", "queries" and "any other module that only depends on it".<br>

<br><br>
Cross points can create new branches of modules that depend on that branch and only. If a module depends on 'domain.queries', then a branch from queries should be created.<br>
Dependencies are structured like a tree so the branching systems with cross points match the tree struct.<br>
Based on DDD(domain is the core) communication modules like web_account depend on the parent which is queries.<br>
<br>

<br>
An "interface module" may have multiple parents and it is domain's developer to merge approprietly.
An "implement module" has only one parent.
<br>

<br>
Queries:<br>
It is an "interface module". This is cross point to create<br>
new "database_imp", "web_service" and "any other form of service modules".<br>


<br>
web_service_modules:<br>
It is created for the last parent towards domain (queries).<br>
This is ment to be installed as a jar and then have the scanner's jar have it.<br>

<br>
Modular_Monolith:<br>
It contains the merges of all branches and helps with the "mvn install".<br><br>

<br>
serviceModule1+serviceModule2+...-branch:<br>
It is created from main. The only changes are in the scanner's pom where you define runtime services to be packaged in the final jar.<br>
Always "mvn install" to overwrite the modular_monolith's main module compiled jar. (The mvn repo will be located in an enviroment like Github Actions)<br><br>

Roles of developers:<br>
1)The regular developer who implements the "imp modules" creates(if not exists) from the appropriate cross point a branch that maps to his working module.<br>
He only pulls from the parent branch. After pushing (only his barnche's change) he also needs to merge the modular_monolith with his branch.<br>
(which the only confflict will be in the project's parent pom, to add the potential missing module)<br>
If changes have been made in the parent module the push must be rejected.<br>
2)The domain developer who changes the "interface modules" based on requirements. He also creates the deployable branches.<br><br>

For deployment you only have to "mvn install" from modular_monolith branch and "mvn install" from the service_to_be_deployed branch. you then deploy the main.jar.
