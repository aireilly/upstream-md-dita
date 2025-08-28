# Upstream markdown source publishing downstream DITA output

Quick POC demonstrating markdown, mkdocs and building DITA output from the markdown source.

Docs are built and deployed using a standard mkdocs set up: 

A separate CI job publishes a DITA OT build of the `example.mditamap`.

## Notes

* The DITA OT [org.lwdita](https://github.com/jelovirt/org.lwdita) plugin has a bug related to transforming markdown into task DITA topics. See https://github.com/jelovirt/org.lwdita/pull/242. This repo includes a hotfix build of the plugin.
* DITA topics generally require a 1:1 topic:file set up which is not generally how markdown docs site operate. Generally, markdown files in mkdocs sites have multiple sub-sections.
* You "type" the markdown topic for DITA conversion with header attributes:

    ```markdown
    # Installing using the CLI {.task}
    ```

    This is supported in a md > dita build but only in base DITA topic types.

* For ease of use, it's probably better to ignore DITA topic typing in markdown topics. 
* For the DITA build to succeed, the markdown is necessarily quite constrained. The DITA build can easily be broken. For example, in the following markdown snippet, the DITA OT build is broken by the extra line after the fourth step: 

    ```markdown
    4. Verify that the installation succeeded by inspecting the CSV resource in the `openshift-numaresources` namespace. Run the following command:
    
       ```cmd
       NAME                             DISPLAY                  VERSION   REPLACES   PHASE
       numaresources-operator.v4.19.2   numaresources-operator   4.19.2               Succeeded
       ```
    
    This breaks the build
    ```
    
    The error is not intuitive:
    
    ```cmd
    Error: file:/home/aireilly/upstream-md-dita/out/docs/task-installing-nro.dita:24:162: [DOTJ088E] XML parsing error: The content of element type "taskbody" must match "(prereq?,context?,(steps|steps-unordered)?,result?,tasktroubleshooting?,example?,postreq?)".
    ```