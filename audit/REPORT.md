# yAcademy RLN Review <!-- omit in toc -->

**Review Resources:**

None beyond the code repositories.

**Auditors:**

 - Chen Wen Kang

## Table of Contents <!-- omit in toc -->

TODO_Insert_TOC

## Review Summary

**TODO_protocol_name**

TODO_protocol_name provides TODO_explain_protocol_purpose.

The contracts of the TODO_protocol_name [Repo](TODO_github_URL) were reviewed over TODO_days_total days. The code review was performed by TODO_number_of_auditors auditors between TODO_start_date and TODO_finish_date, 2023. The repository was under active development during the review, but the review was limited to the latest commit at the start of the review. This was commit [TODO_hash](TODO_github_URL_to_hash) for the TODO_protocol_name repo.

## Scope

The scope of the review consisted of the following circuits at the specific commit:

- `rln.circom`
- `utils.circom`
- `withdraw.circom`

After the findings were presented to the TODO_protocol_name team, fixes were made and included in several PRs.

This review is a code review to identify potential vulnerabilities in the code. The reviewers did not investigate security practices or operational security and assumed that privileged accounts could be trusted. The reviewers did not evaluate the security of the code relative to a standard or specification. The review may not have identified all potential attack vectors or areas of vulnerability.

The auditors make no warranties regarding the security of the code and do not warrant that the code is free from defects. The auditors do not represent nor imply to third parties that the code has been audited nor that the code is free from defects. By deploying or using the code, TODO_protocol_name and users of the contracts agree to use the code at their own risk.


Code Evaluation Matrix
---

| Category                 | Mark    | Description |
| ------------------------ | ------- | ----------- |
| Access Control           | Good | TODO |
| Mathematics              | Good | TODO |
| Complexity               | Good | TODO |
| Libraries                | Average | TODO |
| Decentralization         | Good | TODO |
| Code stability           | Good    | TODO |
| Documentation            | Average | TODO |
| Monitoring               | Average | TODO |
| Testing and verification | Low | More test recommended  |

## Findings Explanation

Findings are broken down into sections by their respective impact:
 - Critical, High, Medium, Low impact
     - These are findings that range from attacks that may cause loss of funds, impact control/ownership of the contracts, or cause any unintended consequences/actions that are outside the scope of the requirements
 - Gas savings
     - Findings that can improve the gas efficiency of the contracts
 - Informational
     - Findings including recommendations and best practices

---

## Critical Findings

None.

## High Findings

None

## Medium Findings

None.

## Low Findings

### 1. Low - Missing non-zero check and range check for public input `x`

`rln.circom` has a public input `x` that is used to generate the identity commitment. The circuit does not check if the public input `x` is non-zero nor within a certain range, which will allow the prover to accept any value for `x` and generate a valid proof that might expose additional information.

#### Technical Details

When given a value of 0 for `x`, the circuit will assign `y` with the value of `identitySecret + a1 * x`, which resolves to `identitySecret`, exposing the user secret.

Given a large value like `p` for `x`, the `y` value might overflow as the calculation of `identitySecret + a1 * x` will result in a value larger than the field size.

This is a non-issue for verification as the verifier will only accept valid `x` value to the proof.

#### Impact

Low. Given that the proof generation is done on the client side, the client can ensure that the public input `x` is non-zero and poseidon hashed before generating the proof.

#### Recommendation

There are 2 possible solutions to this issue:

1. Perform the hashing of `x` in the circuit.
2. Add a check in the circuit to ensure that `x` is non-zero.

## Gas Savings Findings

### 1. Gas - TODO_Title

TODO

#### Technical Details

TODO

#### Impact

Gas savings.

#### Recommendation

TODO

## Informational Findings

### 1. Informational - Unused Public Inputs Optimized Out

`withdraw.circom` has a public input `address` that will be optimized out by the compiler, as its not used as a constraint in the circuit.
This is not an issue as this circuit is used for utility, but if the circuit is used as a standalone circuit, the verifier will accept any value for public input `address`, allowing proof to be reuse for any `address` value.

#### Technical Details

None.

#### Impact

Informational.

#### Recommendation

Remove the public input `address` from the circuit.


### 2. Informational - Misleading Naming Convention for Utility Circuits

The `withdraw.circom` does not involve the generation of a proof to withdraw funds from the contract. Instead, it is a utility circuit that is used to generate the identity commitment from a private identity hash. The naming of the circuit is misleading.

#### Technical Details

NA.

#### Impact

Informational.

#### Recommendation

Rename the circuit to `identity_commitment.circom` to better reflect its purpose.

#### Developer Response

None

## Final remarks

TODO
