# v0.11.1 vs v0.12 - Don't trust, verify
Date: **2025-07-23**

As detailed in the [motivation
document](https://github.com/rgb-protocol/.github/blob/main/MOTIVATIONS.md),
our decision to continue RGB v0.11.1 development in a new GitHub organization
was driven by a combination of organizational and technical concerns. This
document focuses on the **technical reasons** why we chose to continue
development based on version **v0.11.1**, rather than adopting the new
**v0.12** version, and aims to address some of the misconceptions we noticed
about the v0.11.1.

## About v0.12 features
Below, we respond to the main claims made in the [v0.12 release
announcement](https://rgb.tech/blog/release-v0-12-consensus/).

### zk-STARKy RGB
This is arguably the most interesting claim of v0.12, but currently it remains
more of a **marketing narrative** than a usable feature.
- As of now, only preparatory work to make RGB more likely to be compatible
  with ZK proofs has been done, but actual integration of ZK into RGB still
  needs to be done and will require substantial development
- Therefore, there is no evidence yet that ZK integration will be possible
  **without breaking changes** to the protocol
- As such, it cannot be considered a feature at this stage

### Protocol simplification

> much reduced attack surface

While simplification is generally a positive goal, we have **concerns** about
the implementation of this claim in v0.12:
- Large portions of consensus-critical code were either moved to other
  repositories or removed entirely, without accompanying technical
  justification or validation
- A direct comparison of the [v0.12 rgb-core source
  code](https://github.com/RGB-WG/rgb-core/tree/v0.12/src) with that of
  [v0.11.0-beta.9](https://github.com/RGB-WG/rgb-core/tree/v0.11.0-beta.9/src)
  (approved by the LNP/BP Standards Association at the time) reveals a drastic
  reduction in code, which may appear beneficial but does not guarantee
  increased safety or correctness
- In fact, essential features critical for **schema expressiveness and
  privacy** capabilities, such as multiple transitions per contract and
  concealed transitions, were removed in v0.12
- We calculated **~7% of code reduction** in v0.12 compared to v0.11.1
  (considering for v0.12: rgb-core, rgb-std, rgb, rgb-interfaces,
  client_side_validation, sonic, ultrasonic, aluvm, zk-aluvm, bp-core and
  bp-std; considering for v0.11.1: rgb-consensus, rgb-ops, rgb-api,
  rgb-schemas, client_side_validation, aluvm, bp-core and bp-std). However,
  after the planned unused code removal, v0.11.1 will become smaller than the
  current v0.12

Our approach has been guided by the principle: "**Make it work, make it right,
make it fast**".

To ensure the protocol **works correctly**, our priority has been to expand
integration test coverage before any major refactoring. This allows us to
**improve the codebase safely** and with confidence, while minimizing the risk
of regressions. In line with this approach, we have intentionally preserved
some **unused or legacy code** in v0.11.1 to **minimize diffs** and **simplify
the review of our PRs**, as previously agreed with Maxim. Code cleanup is a
**non-breaking** and lower-priority task, which we plan to complete after the
test suite is strengthened and before any further audit.

In contrast, v0.12 involved an **extensive rewrite of core logic without a
sufficient suite of integration tests**, which increases the risk of
introducing subtle bugs and reduces confidence in correctness.

> better auditability

We respectfully **disagree** with this claim.
- In our view, v0.12 is **harder to audit** due to increased **fragmentation**:
  consensus and validation logic are now spread across multiple repositories
- Centralizing and isolating critical logic, as in v0.11.1, is more conducive
  to rigorous review and testing
- As previously pointed out, code reduction was not significant, therefore it
  should not be considered as a reason for better auditability

> better performance

This claim is **unsubstantiated**:
- The v0.12 release does not provide benchmarks, test results, or technical
  metrics demonstrating measurable performance gains
- Without transparent data or reproducible test cases, we cannot evaluate this
  claim

> better developer experience which opens a way for building simpler UX with
> improved user experience

This is a **subjective assertion**, and our experience does not align with it.
- A significant number of applications and libraries have already integrated
  v0.11.1
- v0.12 introduced **incompatible changes**, requiring major refactoring
  efforts and forcing **existing integrations** to be largely rewritten
- It also remains to be seen how hard **new integrations** from scratch would
  be. When it comes to v0.11.1, the existence of higher-level projects like
  rgb-lib allows fast paths to adoption even for developers that don't already
  understand the intricacies of RGB low-level libraries, including support for
  programming languages other than Rust

In our view, this regression in compatibility **negatively impacts developer
experience**.

### Seal unification
This simplification is implemented in v0.11.1 as well.

### Removal of Pedersen commitments and Bulletproofs
Removed in v0.11.1 as well.

### State unification
We intentionally kept states separated in v0.11.1. This does not limit functionality.

### No more schemata
We took a **different approach** by removing interfaces while retaining
schemata in v0.11.1, giving contract developers **greater flexibility** when
designing schemas. We see this as more aligned with innovation and long-term
maintainability.

### Removed interfaces and implementations
Removed in v0.11.1 as well.

### Single-blockchain protocols
This simplification is implemented in v0.11.1 as well.

### No more blank state transitions
v0.11.1 replaced blank transitions with explicit schema-defined transitions,
leveraging `default_transition` for automatic behavior.

In contrast, v0.12 removes blank transitions due to the
single-transition-per-contract limitation, reducing flexibility and privacy.

### Invoicing improvement
We do not observe any functional improvement compared to v0.11.1. The invoicing
capabilities seem equivalent.

### Multiple-asset contracts
Fully supported in v0.11.1 via schema design. This is not a new capability
introduced by v0.12.

### Payment scripts
The same payment flows presented in v0.12 are already **possible without custom
scripts** in v0.11.1, using existing schema logic and transition composition.

### Re-org support
Correct handling of reorgs is fully supported in v0.11.1, it has been one of
the earliest focuses of our work.

### Consignment streams
A compatible and non-breaking enhancement planned in our roadmap for v0.11.1.

### Consignment verification ("order of magnitude" speedup)
This claim is misleading. Queries for transaction data to indexers were
stripped away from consignment validation, hence the speedup. However, they are
still required in order to protect from malicious actors, so total validation
time is still bounded by blockchain data retrieval. Moreover, the described
refactor is **entirely compatible with v0.11.1** and can be integrated without
breaking changes.

### Contract data are no more kept in memory
This is a **planned optimization** also in v0.11.1, which will be addressed
together with streaming consignment processing, and does not require breaking
changes.

### Persistence performance boost with NoSQL
This is already achievable in v0.11.1. However, we prioritize **developer
flexibility** and plan to provide a reference implementation using a SQL
database.

### Test coverage
- **Unit test coverage** in v0.11.1 is expected to significantly improve after
  unused code is removed, a cleanup step we intentionally delayed to simplify
  review by minimizing diffs
- Our focus has been on **validation attack tests**, which we believe are **far
  more critical** for protocol security than high unit test coverage. These are
  actively developed in
  [rgb-tests](https://github.com/rgb-protocol/rgb-tests/blob/master/tests/validation.rs)
- Given the **multi-repo nature** of the RGB ecosystem, **integration tests**
  offer a more reliable testing strategy compared to isolated unit tests
- The unit tests currently present in rgb-core (in
  [`seals.rs`](https://github.com/RGB-WG/rgb-core/blob/v0.12.0/src/seals.rs#L131-L178)
  and
  [`verify.rs`](https://github.com/RGB-WG/rgb-core/blob/v0.12.0/src/verify.rs#L313-L724))
  are **minimal** in scope and, although they achieve high code coverage, do
  not adequately cover **consensus-critical logic**, including potential
  validation attack vectors
- Unit tests are more useful for **anti-regression** purposes when refactoring
  code than for security guarantees, therefore they are a misleading metric to
  measure security

## Addressing some claims
Here are some clarifications, with relevant technical context, about the main
critiques that have been raised about RGB v0.11.1 in public discussions and
channels.

### "v0.11.1 includes heavy modifications to the consensus"
The changes introduced in [the
diff](https://github.com/rgb-protocol/rgb-consensus/compare/v0.11.0-beta.9...0.11.1-rc.3)
between v0.11.0-beta.9 and v0.11.1 are:
- Missing validation checks to prevent known attack vectors
- A reworked transition bundle design that enhances **privacy** by allowing
  transitions to spend **individual assignments** rather than all outputs from
  a UTXO
- Internal improvements such as **seal unification** and **extensions removal**

These changes are **incremental and targeted**, not a rewrite, and focus on
**security and privacy**, unlike the broad and untested rewrites in v0.12.

### "v0.12 has better documentation"
- It is unclear how documentation quality is being measured
- v0.11.1 retains the same code-level documentation as v0.11.0-beta.9, which we
  updated and will keep further improving
- Our team prioritized **consolidating protocol documentation** in a single,
  accessible source, [docs.rgb.info](https://docs.rgb.info/)

### "All assets issued on 0.11.1 are anyway centralized" ([source](https://t.me/rgbtelegram/53213))
This is **false**. Just as in v0.12, while asset issuance is centralized,
holders are anonymous post-issuance: the issuer has **no knowledge** of asset
holders and **no control** over asset transfers after initial distribution.
This makes RGB qualitatively different from a centralized database.

### "All existing RGB-LN products are centralised and hold users’ private keys" ([source](https://t.me/rgbtelegram/53839))
This is also **false**.
- v0.11.1 and products built on top of it don't require, nor even suggest, to
  hold user private keys; all projects are designed with privacy and
  self-custody in mind
- Projects like [Kaleidoswap](https://kaleidoswap.com/) already allow RGB-LN
  interactions via **non-custodial setups**, such as a locally running RLN node

### "v0.12 uses fewer resources"
- **No benchmarks** or technical data have been provided to support this claim
- It's unclear how memory or performance metrics were evaluated across versions
- v0.11.1 has not yet been optimized and many improvements are known to be
  possible and already planned

### "v0.12 has no known bugs or issues"
This claim is **misleading** due to a lack of test coverage visibility and
real-world usage:
- The PR updating rgb-tests to v0.12 does **not run tests in CI**, making it
  difficult to confirm correctness
- Critical tests were **disabled** using `#[ignore]`, including:
    - `ln_transfers` (Lightning-style RGB transfers)
    - `collaborative_transfer` (2-wallet transfers like CoinJoin)
    - `mainnet_wlt_receiving_test_asset` (rejecting testnet assets on mainnet)
    - UDA support tests

  See the screenshot below for a snapshot of `TODO`, `FIXME`, and `#[ignore]`
usage in that branch:
![git grep screenshot](/assets/git_grep_screenshot.png)
- Furthermore, many integration tests from v0.11.0-beta.9 were removed or
  rewritten in v0.12, reducing confidence in detecting regressions

### "v0.12 is more stable and production-ready"
We respectfully **disagree**:
- As of today, only rgb-core has been declared as ready, the rest of the
  stack remains without production-ready releases
- Other essential crates are still being finalized at the time of these claims
- Many tests remain incomplete or ignored

### "Security concerns in v0.11.1 were ignored"
- We didn’t ignore security concerns. As pointed out in
  [rgb-core PR #292](https://github.com/RGB-WG/rgb-core/pull/292), in [rgb-core
  PR #293](https://github.com/RGB-WG/rgb-core/pull/293) we addressed the
  security issues without removing core privacy features
- [rgb-core PR #292](https://github.com/RGB-WG/rgb-core/pull/292) is a deep
  rewrite of core validation logic, among other things it replaces AluVM with a
  whole new validation system for contract transitions, which looks more
  limited in capabilities
- Maxim eventually stopped responding to the discussion, without even providing
  rationales for the changes in order to properly discuss them nor completed
  the work to prove no existing test stopped passing
- The [rgb-tests
  history](https://github.com/rgb-protocol/rgb-tests/commits/master/) shows
  that for every bug we found we created a commit with a test reproducing it,
  followed by a commit that updates the submodules to the commit(s) fixing it.
  While we are not requiring bug reporters to be as rigorous, it would be nice
  if attack claims were at least detailed enough to allow us to reproduce them
- We provided a
  [test](https://github.com/St333p/rgb-tests/blob/24ee570715c7e18d5ede7aa5ed38fa305c2b0086/tests/transfers.rs#L2258)
  demonstrating the [reported](https://t.me/rgbtelegram/54173) "attack in
  v0.11.1 on transition bundles" was correctly detected by the validation code
  since concealed transitions were not allowed
- We then brought back concealed transitions with a new structure of the input
  map and provided a
  [test](https://github.com/rgb-protocol/rgb-tests/blob/4e4325e60bdfa4f706d7fc9d306de79f45cc8fcc/tests/transfers.rs#L2565)
  demonstrating the attack is still not possible
- The [reported](https://t.me/rgbtelegram/54173) "attack on PSBTs" is
  impossible. The JSON serialization of the PSBT is used only for debug
  purposes. PSBTs are and should otherwise always be serialized using the BIP
  174 consensus rules

### "Smart contracting capabilities were removed in v0.11.1"
This is **false** and this claim has not been substantiated.

## Broader concerns about v0.12

### v0.11.1 is already known and audited by the Bitfinex team
Our team has spent **several years** working with Maxim and auditing the RGB
codebase as it evolved up to v0.11.0-beta.9. Based on this experience:
- Every large rewrite in RGB’s history (v0.11, v0.10, v0.9, …) introduced bugs
  and regressions
- Adopting v0.12, with its major conceptual changes, would require at least
  **months** to learn, audit and extensively test it all again
- For these reasons, we believe continuing from v0.11.1 (which we know,
  understand, and have tested) is the **safer and more responsible** path
  forward

### Governance and transparency concerns in RGB-WG and related GitHub orgs
RGB is **not a one-man project**. Its design has involved:
- Regular developer calls
- Contributions from multiple companies
- An active ecosystem of builders and researchers

However, the working environment within GitHub and Telegram groups has often
been perceived by multiple parties as **unwelcoming**, especially toward
external contributors:
- Constructive critiques have frequently been **dismissed** without adequate
  technical explanation
- **Appeal to authority** has often been used to deal with open discussion and
  peer review
- v0.12 was developed as a major rewrite **without prior discussion** with the
  companies funding or building on RGB
- The rewrite was not openly and collaboratively designed, there have been no
  proposals and community feedback has not been possible. Similar conditions
  affected previous cycles and led to **major regressions**
- Multiple contributors have had their PRs **later rewritten** without any
  logical change (e.g.
  [bp-wallet#63](https://github.com/BP-WG/bp-wallet/pull/63),
  [rgb-interfaces#1](https://github.com/RGB-WG/rgb-interfaces/pull/1),
  [rgb#145](https://github.com/RGB-WG/rgb/pull/145),
  [rgb-core#1](https://github.com/RGB-WG/rgb-core/pull/1)), not due to
  technical issues, but often apparently to keep code ownership, effectively
  **overwriting contribution history**

This kind of environment **discourages collaboration**, weakens review
processes, and ultimately hurts the robustness and growth of the ecosystem.

RGB is not a personal project, and its development should reflect that. A more
inclusive and open process is not only healthier for the community but also
critical for building secure, production-ready software.

### Lightning Network (LN) compatibility unproven in v0.12
Despite being a core use case of RGB, v0.12 has **not yet been tested or
integrated** on Lightning:
- Critical LN-related integration tests are currently ignored in the v0.12 test
  suite
- No implementations of RGB-over-LN are known to use v0.12

In contrast, **v0.11.1 is already integrated** in RLN and there are products
(e.g. [Kaleidoswap](https://kaleidoswap.com/),
[Lnfi](https://www.lnfi.network/) and
[Thunderstack](https://thunderstack.org/)) using it, which are being actively
**tested in real-world scenarios**.
