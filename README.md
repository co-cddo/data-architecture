# data-architecture
data architecture team docs as code and diagrams as code public repository.

## Publish to public repository

Resources in this repository are published versions of private resources published using a GitHub workflow action. By default all PlantUML files ending `.puml` are published here, along with any files which end with `-p.*` .

The publish action works by using the [Copy Cat action](https://github.com/andstor/copycat-action) to checkout all files in the private repository and then copy only those matching the file pattern and push them to the public repository.

## Including published PlantUML diagrams in markdown

By publishing PlantUML files to the public repository they can be included in markdown documents as images:

```markdown
![Test Plant UML Diagram](http://www.plantuml.com/plantuml/proxy?cache=no&src=https://raw.githubusercontent.com/<github username>/<public repository>/<branch>/test.puml)
```

## Referencing PlantUML includes

It is sometimes useful to re-use and compose PlantUML diagrams using the `!include` [pre-processor directive](https://plantuml.com/preprocessing#393335a6fd28a804). When viewing and editing the PlantUML file locally (e.g. through VS Code using the PlantUML add-in), the include directive works relative to the current PlantUML file. However when viewing a markdown file on GitHub, that references a PlantUML diagram using the above approach, these relative include directives cannot be parsed and the diagram will not render.

In order to retain relative paths when working locally, but still enable the diagrams to be viewed through GitHub, an additional GitHub action is required to rebase the relative include paths to absolute paths referencing the PlantUML files hosted in the public GitHub repository.

So given a local PlantUML file like this:

```
@startuml
!include user-types.puml
@enduml
```

Will be rebased in the public GitHub repo to be:

```
@startuml
!include https://raw.githubusercontent.com/<github username>/<public repository>/<branch>/user-types.puml
@enduml
```

This [rebase includes github action](/../../../data-architecture/blob/main/.github/workflows/rebase-includes.yml) does the necessary find and replace using a [python script](/../../../data-architecture/blob/main/.github/workflows/rebase-includes.py).
