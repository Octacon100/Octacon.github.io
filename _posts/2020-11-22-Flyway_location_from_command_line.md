---
layout: post
title: Changing Flyway script locations from the command line
tags: flyway CI CD
published: true
---

If you are looking for an open source data base source control and migration tool, I have found [Flyway](https://flywaydb.org/) to be an excellent tool that works with both Oracle and Microsoft SQL Server, and many more. How and why is a topic for later, but for now, here's a quick tip on how to change the flyway migrate call to select different locations from the command line or any scripting language.

Flyway's [Migrate](https://flywaydb.org/documentation/usage/commandline/migrate) command, usually uses whatever is in the [flyway_install]/conf/flyway.conf file for its configuration settings, and you can either create multiple configuration files for different deployments, or enter configuration options on the command line. This is useful in settings where you may want to have certain code be deployed in certain environments, like only installing unit tests on you development environment, or having different synonyms for a local environment versus a remote environment.

In this case, you'll want to change the "[locations](https://flywaydb.org/documentation/configuration/parameters/locations)" option. It's a comma delimited option that allows you to use both classpath locations and filesystem locations, along with cloud locations like Amazon s3 buckets and Google Cloud Storage. Today, I'll be using filesystem locations.

Here's an example:

	flyway -locations=filesystem:/../../releases,filesystem:/../../unit_tests,filesystem:/../../repeatable migrate
    
    #TODO: Check on this when you get home.

A couple of things here, this is currently set up to look outside of the Flyway installation directory to get the code to apply to the database, in this case, all of the sql scripts within the releases directory, the unit_tests directory, and the repeatable directory. This would include all files within directories under those directories as well, as long as they are not hidden.

From here, it's easy enough to change your call depending on parameters you set up in a script. I'll use a powershell script as an example:

    $flywayLocations = "-locations=filesystem:/../../releases"
    #Add extra locations depending on parameters.
    if $unitTest -eq "true"
    {
        $flywayLocations = $flywayLocations + ",filesystem:/../../unit_tests"
    }
    if $repeatable -eq "true"
    {
        $flywayLocations = $flywayLocations + ",filesystem:/../../repeatable"
    }
    #Then run flyway
    flyway $flywayLocations migrate

You can put that within a powershell script and call the powershell script with different parameters, making deployment to different environments much easier, and giving you more control on the release process in a lightweight way. This is helpful when you want to only run version updates, if your database is not fully in source control yet, or to skip certain code as needed.

Hope that helps.
