# Experiment Proposal: [Title of the Experiment]

## 1. Status

[Proposed]

## 2. Context

[Describe the context and background for the experiment. This could include any relevant technical or business factors that might affect the experiment.]

## 3. Hypothesis

Theoretical scenario where we're pushing the limits of ArgoCD's ApplicationSets without regard for the built-in safeguards

## 4. Experiment Design

A recursive loop with ArgoCD ApplicationSets using a combination of generators:

List Generator:
    Starts with a list generator that creates an initial set of applications.
    Each application in this list points to a different path in a Git repository.

Git Directory Generator:
    In each path specified by the list generator, have an ApplicationSet definition that uses a git directory generator.
    These ApplicationSets are designed to create new applications based on different directories in the same or other Git repositories.

Cluster Generator:
    Each of the applications created by the directory generator then uses a cluster generator.
    This could theoretically create new applications for each cluster it's deployed in, and each of these applications could have its own ApplicationSet definitions, triggering more generation cycles.

Template:
    In the template section of each ApplicationSet, would specify the details for the creation of the next set of applications, including the source path (which might contain new ApplicationSet definitions).

yaml proposal

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: initial-appset
spec:
  generators:
    - list:
        elements:
          - cluster: dev
            url: https://example.com/dev-repo.git
          - cluster: prod
            url: https://example.com/prod-repo.git
  template:
    metadata:
      name: '{{cluster}}-application'
    spec:
      project: default
      source:
        repoURL: '{{url}}'
        targetRevision: HEAD
        path: 'path-to-next-appset/{{cluster}}'
      destination:
        server: 'https://kubernetes.default.svc'
        namespace: '{{cluster}}'
```


## 5. Decision

[Describe the decision that will be made based on the results of the experiment. This could be a decision to implement a new feature, change an existing feature, or maintain the status quo.]

## 6. Consequences

[Describe the consequences of the decision. This should include any potential impacts on the system architecture, performance, or user experience.]

## 7. References

[List any references or resources that were used in the preparation of this proposal.]
