# Chapter 8: Release Engineering
## Overview
- Release Engineering is a relatively new practice in software engineering that encompasses building and releasing software.
- This practice includes a good understanding of source code management, compilers, build configuration languages, build tool automation, package management, testing philosophy, configuration management, etc.
- Operating reliable software starts with being able to reliably build and release software in a reproducible, automated fashion.
- Release Engineering often defines best practices for every aspect of the build and release process, creating a Golden Path for product development teams. This can include things like compiler flags, formats for build tagging, etc.
- When Release Engineering is functioning well product development teams should be able to build and release their software without much manual intervention.

## Philosophy
### Self-Service Model
The role of Release Engineering is to develop best practices and tooling that allows product development teams to easily run their own release process. Ideally, teams can leverage the tooling to automatically build and release software, on some agreed upon cadence, with manual intervention only when problems arise.
### High Velocity
Release Engineering is meant to increase the velocity with which software can be released to users, while still maintaining a high quality bar.
### Hermetic Builds
The build process should be consistent and repeatable, which implies that it is agnostic to the machine on which it is run. Everything needed to execute the build (e.g. compilers, dependencies, etc.) should be known and appropriately versioned.
### Enforcement of Policies and Procedures
Specific actions (e.g. approving code changes, creating a release, deploying a release, etc.) in the build and release process can only be performed by certain individuals and only through approved processes (e.g. code review).

## Continuous Build and Deployment
### Building
Software can have many kinds of build artifacts (e.g. binaries, images, etc.). Each change to the codebase can trigger the build of one to many of these artifacts depending on the product and how the team intends to release the software to users.

### Branching
While there are different types of branching strategies, each with its own pros and cons, it is important to have a consistent branching strategy that enables for release candidates to maintain an exact set of contents.

### Testing
Testing is paramount in detecting regressions and failures quickly. Ideally, each new submission to the codebase is thoroughly vetted by running a set of tests that is identical to the tests that gate a release. When a new release is cut from the mainline (i.e. the last continuous build to pass all tests), all tests should be re-run to ensure that the release candidate passes muster on its own.

It is often advantageous to supplement continuous build testing with additional stress, performance, failure, canary, etc. testing of packaged build artifacts.

### Packaging
Once a release candidate has been properly validated it should be packaged for distribution. This often includes naming, versioning, and signing the package.

### Deployment
Deployments can come in many flavors depending on the risk associated with rolling out the new software. The risk can be dependent on the type of environment, the criticality of the service, the level or usage, etc. Deploying new software can be done immediately, or progressively over time. In all cases, the process should be reversible, should some unexpected behavior arise.

## Configuration Management
Configuration can be just as challenging to the stability of software as the code itself. Many large scale failures have been attributable to poor configuration management practices. Having a good strategy for managing configuration and even being able to reproduce configurations for particular builds can have a large impact on building reliable software.

## Conclusions
Proper Release Engineering practices can make for operating much more reliable software. These practices should answer questions like:
- How are packages versioned?
- Should software be continuously built and deployed? Or periodically?
- How often should releases occur?
- What is the strategy for managing configuration?
- What release metrics are of interest?

Release Engineering is often applied to software development practices as an afterthought, but it is a critical piece of operating reliable software. As such, teams should appropriately account and budget for release engineering practices at the beginning of the product development lifecycle. Not all project teams may not want to invest in Release Engineering, so it is really up to the team when to bite the bullet and incorporate Release Engineering practices.