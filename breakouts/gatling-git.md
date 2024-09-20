# GATLING

- Load testing tool (initially for HTTP protocol, but is agnostic)
- Extensions are done through a DSL, there is a Scala and a Java API
- The Git protocol impl was merged upstream

## Concepts

- Scenario
     - User behavior (what is the path of the user)
     - Define the steps of a single user
- Injection profile
    - Describe how users are injected in your system (rate limiting, load balancing etc.)
    - You can define the number of concurrent users
    - How they ramp up and down, etc.
- Simulation
    - Description of the load test
    - Define how user populations will run (scenario, injection rate)
- Session
    - Users can save data that can be used in a subsequent request
- Feeders
    - API to inject data from an external source
    - e.g. list of repositories to be fed to the simulation

## Notes

(gatling-sbt-gerrit-test is their vanilla repo for load testing a git repo)

- Recording production to replay what happens through a feeder is possible
- Possible to randomize pauses with some kind of standard deviation
- Reporting is an HTML dashboard, but there is also a JSON file to get all the data
- You can setup a fleet of clients for many machines, but only the Enterprise version can collate the multiple report
