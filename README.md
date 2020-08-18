[![License: MIT](https://img.shields.io/badge/License-MIT-orange.svg)](https://opensource.org/licenses/MIT)
![PMD Best Practices](https://github.com/philips-software/cerberus/workflows/PMD%20Best%20Practices/badge.svg)
![Mutation Analysis 96%](https://github.com/philips-software/cerberus/workflows/Mutation%20Analysis%2096%25/badge.svg)
![Duplicate Blocks 37 Tokens](https://github.com/philips-software/cerberus/workflows/Duplicate%20Blocks%2037%20Tokens/badge.svg)
![Checkstyle Adherence](https://github.com/philips-software/cerberus/workflows/Checkstyle%20Adherence/badge.svg)
![Compile And Assemble](https://github.com/philips-software/cerberus/workflows/Compile%20And%20Assemble/badge.svg)
![SpotBugs Scan](https://github.com/philips-software/cerberus/workflows/Spotbugs%20And%20Scan/badge.svg)

# Cerberus

Cerberus is a tool to measure code quality parameters, which can be used as a watch dog to observe code quality parameters like copy paste errors, suppressed warnings etc., and also to gate builds when the allowed thresholds for the parameters being observed is breached.

### Why another tool? 
We asked ourselves the same question! Why create a new tool when there were many industry standard tools like SonarQube, Coverity, Fortify etc. Here's why we think Cerberus works better for us. 


- No deployment model: Unlike other tools that requires setups, or a "server" and "client" mode of operation, Cerberus is an executable jar. As long as we have a JVM 1.8 or higher, Cerberus works, and needs no deployment!
- Data generation & sharing: Cerberus is lightweight, and mostly generates data in the form of JSON, with customization available on the reporting format. One thing to note however is, Cerberus does not store data. All data generated by Cerberus is locally stored on the machine where it is executed. 
- Business specific metrics: While some of the metrics measured by Cerberus are re-used from components like PMD, CPD etc. some of the metrics measured are business specific, like providing traceability of suppressed warnings annotation in code, providing a facade on top of [CK](https://github.com/mauricioaniche/ck) to offer configurable metrics parameters and diffs. Cerberus takes in many such business requests and builds them for one team, and everyone else using Cerberus gets it for free! 
- Faster capability extension cycles: We also wrote Cerberus to churn code and build faster. Every business request to us has a time-to-market need. With a code base internally owned, we can do internal code-reviews faster and release versions faster to meet the business needs.
- Open tenets: Available industry standard tools have set tenets, which if we want to cross, we may not have options. Our tenet is simple. Let's meet the business needs keeping in mind quality and integrity of the software written. This way, with our tenet, we can continue to explore many options and keep integrating it to Cerberus.
- Reuse first policy: Our intention is not to rebuild another PMD or another SpotBugs. Cerberus will be a facade for such tools so that development team need not work on plumbing each of such independent tools, for different programming languages used in the project and for different projects developed by the team.
- Built for learning & demoing: As part of Software Center of Excellence, we consult with many businesses and guide them on how to gate the code for quality parameters, how to refactor code, how to create more readable and maintainable code. Cerberus is built keeping all the principles of software craftsmanship that SWCoE suggests, so that we can use Cerberus as an example to show how to integrate the principles of clean-code practices into code. We also wrote Cerberus because we wanted to learn what it's like to write a software that adheres to good code quality standards. 


## How to use Ceberus? 
### Build Cerberus from source
To build Cerberus, you will need a JDK 8 and apache maven installed in your system. 
The final output of build is an executable JAR. We have created maven goals to achieve the same, review our POM file for more details.  

1. Clone the repository
`git clone https://github.com/philips-software/cerberus.git`
2. Build it using the following command
`mvn clean package assembly:single`
The executable jar will be generated in the target folder. 

### Run Cerberus as a command line tool
Once the jar is built, use the jar and execute Cerberus

```
$ java -jar cerberus.project-1.0.0-SNAPSHOT.jar
Usage: Cerberus [COMMAND]
Waking Cerberus to devour bad things in the system
Commands:
  CPD            Detect duplicated blocks of code in your source code
  SWD            Detect all the warnings which are suppressed in your code
  JCMD           Java Code Metrics Detector
  JCMD-DIFF      Java Code Metrics Detector with Diff
  FPM            Find Programming mistakes in code
```

### Usage of FPM 

You can find detailed instructions on  FPM ( Find Programming mistakes )  
[here](docs/FPM.md)

### Usage of JCMD-DIFF

You can find detailed instructions on JCMD-DIFF ( Java Code Metrics Detector with Diff ) along with the explanation for each metric 
[here](docs/JCMD.md)

### Dogfooding 
To evaluate and confirm the quality of code changes made to Cerberus, you should run through all the gates and review the generated reports with:

`mvn clean package checkstyle:check pmd:check pmd:cpd-check pitest:mutationCoverage pitest:report site surefire-report:report`

Afterwards, to use the actual JAR for yourself run the below command by checking out the master branch in this repository with:

`mvn clean assembly:single`

The above command runs following tests

1. Runs Automated tests
2. Code coverage using Cobertura
3. PMD for programming mistakes 
4. CPD for copy paste detection 
5. Mutation testing

We run the same maven goals in our pipeline as well. 

As mentioned above, Mutation testing is integrated into the pom.xml with a defined gating % in the pom.xml. To run the mutation testing alone, run the below command

`mvn compile org.pitest:pitest-maven:mutationCoverage`

### Dockerizing Cerberus

You can also utilize the Dockerfile in the repo to create a Docker image 
and spin up a container to build and run to do so run below commands

`docker build -t cerebrus_image .`

Once the image is built, you can actually spin up the container using the command below

`docker run -ti -v /path/to/Project/Cerebrus:/usr/src cerebrus_image bash`

### Contact 

[Maintainers Of Project](MAINTAINERS.md)


### To Contribute 

You are always free to fork this repository and create a PR to develop branch and ask for reviewer, Once approved it gets merged to develop branch . 
To prevent build breaks run the same maven goals that we run in our pipelines for all quality checks

```
mvn clean package checkstyle:check pmd:check pmd:cpd-check pitest:mutationCoverage pitest:report site surefire-report:report compile assembly:single
```

## License

[MIT](LICENSE)

## Credits and references

1. [PMD]( https://pmd.github.io/ )
2. [CPD](https://pmd.github.io/latest/pmd_userdocs_cpd.html#overview )

## Inspiration 

1. [Facebook Infer ](https://fbinfer.com/)
2. [Bazel Build ](https://bazel.build/)
