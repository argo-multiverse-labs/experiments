# Experiment Proposal: [Title of the Experiment]

## 1. Status

[Proposed]

## 2. Context

[Describe the context and background for the experiment. This could include any relevant technical or business factors that might affect the experiment.]

## 3. Hypothesis

Create a recursive ApplicationSet in ArgoCD that the ApplicationSet generates applications that in turn generate more applications, and so on. 

## 4. Experiment Design


 * Directory Generator: Suppose a directory generator that creates applications based on directories in a Git repository.

 * Cluster Generator: Then, each of these applications uses a cluster generator to create new applications for each cluster it's deployed in.

 * Git Generator: Each of these new applications could, in theory, point to a new Git repository or branch that triggers the original ApplicationSet to generate more applications.

Possible appset-spec yaml

```yaml
apiVersion: argoproj.io/v1alpha1
kind: ApplicationSet
metadata:
  name: recursive-appset
spec:
  generators:
    - git:
        repoURL: 'https://example.com/my-repo.git'
        directories:
          - path: 'applications/*'
  template:
    spec:
      project: default
      source:
        repoURL: 'https://example.com/my-repo.git'
        path: '{{path}}'
        targetRevision: HEAD
      destination:
        server: 'https://kubernetes.default.svc'
        namespace: 'default'
```

## 5. Decision

[Describe the decision that will be made based on the results of the experiment. This could be a decision to implement a new feature, change an existing feature, or maintain the status quo.]

## 6. Consequences

[Describe the consequences of the decision. This should include any potential impacts on the system architecture, performance, or user experience.]

## 7. References

[List any references or resources that were used in the preparation of this proposal.]
