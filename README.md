
# Codacy Metrics Scala Seed

We use external tools at Codacy, this is the library we use across the multiple external tools integrations.
For more details and examples of tools that use this project, you can check
`TODO`.

### Usage

Add to your SBT dependencies:

```
"com.codacy" %% "codacy-metrics-scala-seed" % "1.0.0-SNAPSHOT"
```

You shouldn't worry about the library itself, we use it as a core in our tools,
and everything is well explained in our Docs section.

## Docs

**How to integrate an external metrics tool on Codacy**

By creating a docker and writing code to handle the tool invocation and output,
you can integrate the tool of your choice on Codacy!

> To know more about dockers, and how to write a docker file please refer to [https://docs.docker.com/reference/builder/](https://docs.docker.com/reference/builder/)

We use external tools at Codacy, in this tutorial, we explain how you can integrate the metrics tool of your choice in Codacy.
You can check the code of an already implemented tool and if you wish fork it to start yours.
You are free to modify it and use it for your integration.

#### Requirements

* Docker definition with the tool you want to integrate

#### Assumptions and Behaviour

* To run the tool we provide the configuration file, `/.codacyrc`, with the language to run and optional parameters your tool might need.
* The files to analyse are located in `/src`, meaning that when provided in the configuration, the paths are relative to `/src.

* **.codacyrc**
  * **files:** Files to be analysed (their path is relative to /src)
  * **language:** Language to run the tool

```json
{
  "files" : ["foo/bar/baz.scala", "foo2/bar/baz.scala"],
  "language": "Scala"
}
```

#### Output

You are free to write this code in the language you want.
After you have your results from the tool, you should print them to the standard output in our **Result** format, one result per line.

```json
{
  "filename": "path/to/my/file1.scala",
  "complexity": 1,
  "buildtime": 2,
  "loc": 300,
  "cloc": 320,
  "duplicatedLines": 3,
  "nrMethods": 20,
  "nrClasses": 2,
  "lineComplexities": [
    {
      "line": 2, 
      "value": 3
    }
  ]
}
```

> The filename should not include the prefix `/src/`
  Example:
    * absolute path: `/src/folder/file.js`
    * filename path: `folder/file.js`

#### Submit the Docker

**Running the docker**

```sh
docker run -t \
    --net=none \
    --privileged=false \
    --cap-drop=ALL \
    --user=docker \
    --rm=true \
    -v <PATH-TO-FOLDER-WITH-FILES-TO-CHECK>:/src \
    -v <PATH-TO-CODACY-CONFIG-RC>:/src/.codacyrc \
    <YOUR-DOCKER-NAME>:<YOUR-DOCKER-VERSION>
```

**Docker restrictions**

    * Docker image size should not exceed 500MB
    * Docker should contain a non-root user named docker with UID/GID 2004
    * All the source code of the docker must be public
    * The docker base must officially supported on DockerHub
    * Your docker must be provided in a repository through a public git host (ex: GitHub, Bitbucket, ...)

**Docker submission**

    * To submit the docker you should send an email to `team [at] codacy [dot] com` with the link to the git repository with your docker definition.
    * The docker will then be subjected to a review by our team and you will then be contacted with more details

If you have any question or suggestion regarding this guide let us know.

## Test

> TODO

## What is Codacy?

[Codacy](https://www.codacy.com/) is an Automated Code Review Tool that monitors your technical debt, helps you improve your code quality, teaches best practices to your developers, and helps you save time in Code Reviews.

### Among Codacy’s features:

- Identify new Static Analysis issues
- Commit and Pull Request Analysis with GitHub, BitBucket/Stash, GitLab (and also direct git repositories)
- Auto-comments on Commits and Pull Requests
- Integrations with Slack, HipChat, Jira, YouTrack
- Track issues in Code Style, Security, Error Proneness, Performance, Unused Code and other categories

Codacy also helps keep track of Code Coverage, Code Duplication, and Code Complexity.

Codacy supports PHP, Python, Ruby, Java, JavaScript, and Scala, among others.

### Free for Open Source

Codacy is free for Open Source projects.
