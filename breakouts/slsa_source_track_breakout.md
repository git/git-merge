Open Discussion

* SLSA is meant to establish standards / language to communicate how trustworthy development processes are
* Breaks standards into levels: higher the level, the better the guarantees
* V1.0 has Build track: how builders can secure transforming source code to build artifacts like binaries
* Build level 1: protects against mistakes, documentation
* Build level 2: protects against tampering after the build
* Build level 3: some protections during build with isolation requirements, etc.
* Build track allows for mapping a binary artifact to the source code that produced it
* But raises the question about how to trust the source code
* Q: does build level 3 include reproducible builds?
  * No, this was discussed extensively and was part of the spec pre v1.0
  * On a related note, there’s a separate track being worked on to get hardware attested trust for a build, that provides some similar guarantees
* For the source track, we want to enable repository maintainers to be able to say “repo at revision X meets level Y”
  * This statement would be via a cryptographically signed attestation
  * Which would enable a level of verifiability, inspection of evidence, etc.
* Would any of the SLSA source track requirements help with the xz-utils maintainer incident?
  * The source track would not address this
  * Level 1 today just requires a modern VCS (Git, Mercurial, etc.)
  * Level 2 is branch history: disallow rewriting protected branches
  * Level 3 is provide source provenance attestations that connect a revision to its prior revision
  * Safe expunging process: legal or security reasons where history rewrites are required
* Are there connections to existing standards / compliance requirements?
  * Somewhere in the build track, there was an effort to map requirements to the SSDF so as to say “if you meet this requirement, you can check these SSDF requirements”
* Are there liability concerns by adopting SLSA?
  * Will I be involved in more legal situations as a consequence of adopting SLSA?
* How does this relate to CRA?
  * Not 100% sure about CRA, but maps to requirements in SSDF etc and perhaps similar?
  * Additionally, SLSA is more verifiable with tooling rather than a human-filled checklist
* SLSA \<-\> gittuf
  * SLSA doesn’t want to require a specific toolchain for managing source code
  * Requiring a specific toolchain would also stop the development of new tools
* Is the ambition of SLSA to get bigger players (Google etc) to align on these goals? Startups?
  * Large enterprises and startups
  * Related: do we expect small orgs / engineers to implement controls?
  * One set of SLSA’s consumers are in fact developer tooling / platforms builders who can build requirements into tooling to make it available invisibly to end users
* Individual developers driven repositories
  * A significant number of repositories have one or so significant developers
  * Requirements being imposed on a developer doing this as a hobby
  * Side note: Does the CRA impose more requirements on such hobby developers?
  * SLSA does not set requirements, thinking more along the lines of sane defaults and making it part of the tooling itself
  * Ideal: Most developers needn’t even know SLSA is a thing, just gets built into the tooling.
* Do you foresee something like “this thing we’re bringing into our enterprise is a particular SLSA level?”
  * Yes, that could well be the case
  * If the vendor is a hobbyist, it’s on the enterprise to compensate the hobbyist
* What’s the tooling to verify the attestations for some repo you’re consuming?
  * One option is to trust the attestation to an extent, check the revision matches the attestation, etc.
  * Who makes the attestation (maintainer of repository vs someone else for eg) is an interesting question
* gittuf
  * SLSA isn’t prescriptive
  * You can meet a lot of these requirements by trusting a platform to do them
  * But maybe we can go further and remove the need to trust the platform
  * What does it mean to be SLSA Level 3 if the system enforcing those things can be compromised
* Thoughts on code review
  * Anonymity / pseudo-anonymity are important
  * Individuals can’t meet this requirement
  * In startups: requiring code reviews would be difficult
  * Quality of code reviews vary so the review may not mean much
  * What is reviewed is important to define
  * But code review as a whole is not well defined
* From the consumer’s perspective
  * Would a consumer expect code review from a vendor?
* Automated tests may be better signal than code review

SLSA community: [https://slsa.dev/community](https://slsa.dev/community)
Source Track *Draft:* [https://slsa.dev/spec/draft/source-requirements](https://slsa.dev/spec/draft/source-requirements)
