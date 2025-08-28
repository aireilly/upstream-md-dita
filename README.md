# Upstream markdown source publishing downstream DITA output

Quick POC demonstrating markdown, mkdocs and building DITA output from the markdown source.

Docs are built and deployed using a standard mkdocs set up: 

A separate CI job publishes a normalized DITA build of the `example.mditamap` and topics as a GitHub release.

## Notes

* The DITA OT [org.lwdita](https://github.com/jelovirt/org.lwdita) plugin has a bug related to transforming markdown into task DITA topics. See https://github.com/jelovirt/org.lwdita/pull/242. This repo includes a hotfix build of the plugin that fixes this issue.

* Mkdocs style admonitions are supported.

    ```markdown
    !!! pied-piper "Pied Piper"
    
        Lorem ipsum dolor sit amet, consectetur adipiscing elit. Nulla et
        euismod nulla. Curabitur feugiat, tortor non consequat finibus, justo
        purus auctor massa, nec semper lorem quam in massa.
    ```

* The mkdocs site in this demo is built from the markdown source in the `docs/` folder.

* The GitHub release zip contains a direct normalized DITA build of the md source.

* Although markdown DITA supports concept, task, and reference topic conversion via header attributes, the build necessarily constrains what the writer can add to the file and how they structure content. For upstream writers, this will prove extremely frustrating should we try to constrain content to task/concept/reference.

* You can "type" the markdown topic for DITA conversion with concept, task, or reference header attributes:

    ```markdown
    # Installing using the CLI {.task}
    ```

* DITA topics generally require a 1:1 topic:file set up which is not generally how markdown docs site operate. Generally, markdown files in mkdocs sites have multiple sub-sections.

* For ease of use, it's probably better to ignore DITA topic typing in markdown topics. 

* For the DITA build to succeed for topic types, the markdown is necessarily quite constrained. The DITA build can easily be broken. For example, in the following markdown snippet, the DITA OT build is broken by the extra line after the fourth step: 

    ```markdown
    4. Verify that the installation succeeded by inspecting the CSV resource in the `openshift-numaresources` namespace. Run the following command:
    
       ```cmd
       NAME                             DISPLAY                  VERSION   REPLACES   PHASE
       numaresources-operator.v4.19.2   numaresources-operator   4.19.2               Succeeded
       ```
    
    This breaks the build
    ```
    
    The error is not intuitive, especially for an upstream author:
    
    ```cmd
    Error: file:/home/aireilly/upstream-md-dita/out/docs/task-installing-nro.dita:24:162: [DOTJ088E] XML parsing error: The content of element type "taskbody" must match "(prereq?,context?,(steps|steps-unordered)?,result?,tasktroubleshooting?,example?,postreq?)".
    ```

* I pulled a random large behemoth "nothing type" topic and converted it to md to represent the kind of real world content that we have. Large multi-section markdown files will convert into technically valid DITA XML, but they will not be topic-typed (concept/task/reference).
