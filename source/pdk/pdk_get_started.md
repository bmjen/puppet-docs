---
layout: default
title: "Getting started with the Puppet Development Kit"
canonical: "/pdk/pdk_get_started.html"
description: "Getting started with the Puppet Development Kit, the shortest path to developing better Puppet code."
---

**Note: this page is a draft in progress and is neither technically reviewed nor edited. Do not rely on information in this draft.**

The Puppet Development Kit provides unified tooling to make developing and testing Puppet modules easier. This guide walks you through creating and running unit tests on a module with PDK.

[TODO: Jean get a graphic of the workflow]
[TODO: Jean write a PoC/qsg overview of the workflow creating a new module, in addition to the tasks and concepts below.]

To create a module with the Puppet Developer Kit, you'll:

1. Generate a new module.
1. Validate your module.
1. Unit test your module.
1. Create a class for your module.

{:.concept}
## Generating a new module with pdk

PDK generates the basic components of a new module and sets up the basic infrastructure you need to test your module.

When you create a new module with PDK, the `pdk new module` command asks you a series of questions, sets metadata values based on your answers, and creates a `metadata.json` for your new module. The new module that PDK creates includes all the infrastructure to use the other capabilities of `pdk`.

Each question has a default response that PDK uses if you hit **Enter** to skip the question. [TODO Jean to maybe make this info a table]

* What is your Puppet Forge username? [testuser]
* Puppet uses Semantic Versioning (semver.org) to version modules.
What version is this module? [0.1.0]
* Who wrote this module? [testuser]
* What license does this module code fall under? [Apache-2.0]
* How would you describe this module in a single sentence? [(none)]
* Where is this module's source code repository? [(none)]
* Where can others go to learn more about this module? [(none)]
* Where can others go to file issues about this module? [(none)]

PDK then displays the metadata information that it will use to generate the new module:

``` json
{
  "name": "testuser-hello_module",
  "version": "0.1.0",
  "author": "testuser",
  "summary": "",
  "license": "Apache-2.0",
  "source": "",
  "project_page": "",
  "issues_url": "",
  "dependencies": [
    {
      "name": "puppetlabs-stdlib",
      "version_requirement": ">= 1.0.0"
    }
  ],
  "data_provider": null
}
```

If the information is correct, confirm the module generation with `Y`, and then PDK generates the module. If the information is incorrect, cancel with `n` and start over.

PDK creates a module with directories like this:

```
hello_module/
Gemfile
appveyor.yml
metadata.json

hello_module/manifests:
hello_module/spec:
default_facts.yml
spec_helper.rb

hello_module/templates:

```

After you generate a new module, we suggest validating and testing the module _before_ you add classes or write new code in it. This allows you to verify that the module was correctly created.

{:.task}
### Generate a new module with pdk

To generate a new module with PDK's default template, use the `pdk new module` command.

Before you begin, you should have already installed the pdk package.

1. From the command line, run the `pdk new module` command, specifying the name of the new module.

   ```
   pdk new module hello_module
   ```

1. Respond to PDK's dialog questions in the terminal. To accept the default value for any question, hit **Enter**.

1. If the metadata that PDK displays is correct, confirm with `Y` to generate the module. If it is incorrect, enter `n` to cancel and start over.

Related topics:

* Install PDK
* Add a new resource provider with PDK

{:.concept}
## Generating a class

PDK can generate classes for your new module to configure large or medium-sized chunks of functionality, such as all of the packages, config files, and services needed to run an application. [TODO: Jean to write better].

To define parameters in the class you are creating, specify them on the command line with the `pdk new class` command. You should also specify the values that parameter accepts, and optionally, you can specify parameter's data type. You can provide any number of parameters can be provided on the command line.

If you add parameters with the `pdk new class` command, PDK creates the new class manifest and a test file to validate the expected behavior. If you add classes manually.

For example, to create a new class named `hello_class` and define an `ensure` parameter for the class, run:

``` bash
pdk new class hello_class "ensure:Enum['absent','present']"
```

This command creates a file in `hello_module/manifests` named `hello_class.pp`, with the ensure parameter defined. It also creates a test file in `hello_module/spec/class` named `hello_class_spec.rb`. This test file includes a basic template for writing your own unit tests.

 
{:.task}
## Generate a new class

To generate a new class in your module, use the `pdk new class` command.

1. From the command line, in your module's directory, run:

   ``` bash
   pdk new class hello_class "ensure:Enum['absent','present']"
   ```

{:.concept}
## Testing your module with PDK

PDK provides tools to help you run unit tests on your module and validate your new module's metadata, syntax, and style.

By default, the PDK module template includes tools that can:

* Validate the `metadata.json`.
* Validate Puppet syntax.
* Validate Puppet code style.
* Validate Ruby code style.
* Run unit tests.

The validations included in PDK run quickly, but they provide only a basic check of the well-formedness of the module and the syntax of its files. You do not need to write any tests for this validation.

PDK can also run your unit tests on your module's Puppet code to validate that the resources declared will be included in the catalog. PDK cannot validate changes to the managed system or services.

PDK creates a unit test file when you generate a class with `pdk new class`. This test file, located in your module's `/spec/class` folder, includes a basic template for writing your unit tests. To learn more about how to write unit tests, see [rspec-puppet documentation](http://rspec-puppet.com/tutorial/).

After you've written your unit tests, you can use the `pdk test unit` command to run all of the tests you've included in your module.

We suggest testing and validating your module anytime you are going to modify or add code, to verify that you are starting out with clean code. Then, as you create classes and write other code in your module, validate and test your new code.

Related topics:

* [rspec](http://rspec.info/)
* [rspec-puppet](https://github.com/rodjek/rspec-puppet/)
* [Writing rspec-puppet tests](http://rspec-puppet.com/tutorial/)

{:.task}
## Validate your module with PDK 

To validate that your module is well-formed with correct syntax, run the `pdk validate` command.

1. From the command line in your new module's directory, run:

   ```
   pdk validate
   ```

You should get a result like:

``` bash
Running validations on `hello_module`:
* ruby syntax: OK!
* puppet syntax: OK!
[...]
```

If you get errors on a module you are writing, fix your errors. [TODO Jean, write this better] If you get errors on a freshly created module with no new code in it, [TODO well, then what should they do?]

{:.task}
### Unit test your module with PDK

To unit test your module, use the `pdk test unit` command. This command runs all the unit tests in your module.

Before you begin, you need to have written unit tests for your module. If you are testing a new module that you created with PDK and have not added any code to, you do not need to write unit tests.

1. From the command line, in the module's directory, run all unit tests with:

``` bash
pdk test unit
```

If there are no errors, this returns successfully (exit code 0) with no warnings or errors on 0 examples.

 
