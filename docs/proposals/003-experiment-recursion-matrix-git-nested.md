# Experiment Proposal: [Title of the Experiment]

## 1. Status

[Proposed]

## 2. Context

[Describe the context and background for the experiment. This could include any relevant technical or business factors that might affect the experiment.]

## 3. Hypothesis

Nesting matrix and git generators in an ApplicationSet to create a complex, potentially recursive structure is an interesting theoretical exercise

## 4. Experiment Design

* Matrix Generator:
  * The matrix generator to combine two other generators, creating a Cartesian product of their outputs.
  * Could combine a git generator with another generator (like a list or another git generator).

* Git Generator:
    * The git generator is used to create applications based on the content in a Git repository.
    * Multiple ApplicationSets in different directories or branches within the same or different repositories.

* Nested ApplicationSets:
    * Within the repositories pointed to by the git generator, could theoretically have more ApplicationSet manifests.
    * These nested ApplicationSets could also utilize matrix and git generators, pointing to other repositories or different paths/branches within the same repository.

yaml proposal

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: nested-matrix-appset
spec:
  generators:
    - matrix:
        generators:
          - git:
              repoURL: 'https://example.com/main-repo.git'
              directories:
                - path: 'appsets/*'
          - list:
              elements:
                - cluster: dev
                - cluster: staging
                - cluster: prod
  template:
    metadata:
      name: '{{cluster}}-{{path.basename}}'
    spec:
      project: default
      source:
        repoURL: 'https://example.com/main-repo.git'
        targetRevision: HEAD
        path: '{{path}}'
      destination:
        server: 'https://kubernetes.default.svc'
        namespace: '{{cluster}}'
```

* The matrix generator combines the outputs of a git and a list generator.
* Each application defined in the appsets/* directory of the main repository is templated for 'dev', 'staging', and 'prod' clusters.
* Each of these templated applications could be another ApplicationSet using similar logic, potentially pointing to other repositories or paths.

## 5. Decision

[Describe the decision that will be made based on the results of the experiment. This could be a decision to implement a new feature, change an existing feature, or maintain the status quo.]

## 6. Consequences

[Describe the consequences of the decision. This should include any potential impacts on the system architecture, performance, or user experience.]

## 7. References

[List any references or resources that were used in the preparation of this proposal.]
