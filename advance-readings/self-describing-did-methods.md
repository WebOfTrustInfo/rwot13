# Self-Describing DID Methods

Kevin Dean &lt;[kevin@legreq.com](mailto:kevin@legreq.com)>

## Problem

In the spirit of decentralization, DID method names are generated autonomously by the authors of their respective specifications. Generally, a method name is chosen to represent something about the underlying specification, such as a company name (e.g., `did:iota` developed by [IOTA Foundation](https://www.iota.org/)) or the DID resolution protocol (e.g., `did:web`).

This approach can, naturally, lead to conflicting method names being generated, especially with popular resolution protocols (e.g., `did:etho` and `did:ethr` for Ethereum) or with meaningful words in languages that use the Latin alphabet, as occurs with [memorable domain names](https://www.name.com/blog/the-top-10-most-expensive-domains-ever-sold).

To mitigate this risk, section [8.1 Method Syntax](https://www.w3.org/TR/did-core/#method-syntax) of the DID standard states that a DID method specification _SHOULD_ be registered in the DID Specification Registries.

Again in the spirit of decentralization, registration is not mandatory, so the following scenario is entirely conceivable:

> A loose affiliation of pirates creates the `did:iocane` method for their community, choosing the name based on the powder’s stealthy and useful properties as a poison. Service endpoints published in the DID documents cover types such as `plunder`, `recruit`, and `retire`. To maintain absolute anonymity, the method isn’t registered, and knowledge of it is limited to a need-to-know basis.
>
> Sometime later, a pirate ship makes port in Australia, where the captain negotiates the sale of booty to a local purchaser. The captain has several DIDs, built on registered DID methods, that he can use, and he selects one to conclude the sale. The purchaser presents a `did:iocane` DID, but one that conforms to a registered specification, with the name chosen because of the beauty of the plant by that name and despite the poison that can be derived from it. The pirate’s wallet, which knows only about the private `did:iocane` method, fails to resolve the DID of the purchaser, the sale falls through, the crew mutinies, and the captain is deposed in favor of a Spanish member of the crew.

Another problem with the way that DID methods are defined is the lack of version control. Suppose that a DID method specifies a verifiable data registry that is later found to be vulnerable to some form of attack (e.g., denial of service), serious enough to warrant changing the registry or changing the way in which the registry is accessed. The original DID method name is no longer viable, so a new one needs to be defined. This may be as simple as appending a version to the original name (e.g., `did:example2` as a successor to `did:example`), but there’s no way to show any correlation between the two. Furthermore, assuming that the vulnerability isn’t significant enough to warrant shutting down the original DID method entirely, the developers want to maintain some level of compatibility with the original DID method so that verifier software written for it can be used without modification with the new DID method, giving verifiers time to upgrade without compromising their business processes.

## Requirements

### Collision

DID method names SHALL be generated in such a way that there is a negligible risk of collision.

### Security

DID method names SHOULD be generated in such a way that they can be verified against some external, related content.

### Version Control

DID methods SHOULD advertise compatible DID methods that are usable without modification in processes involving the original DID method.

DID methods SHOULD advertise a replacement DID method that is an upgrade to the original DID method, not guaranteed to be usable without modification in processes involving the original DID method.

#### Semantic Versioning

To simplify the version control requirement further, versioning SHOULD align with the MAJOR.MINOR.PATCH concepts behind [Semantic Versioning 2.0.0](https://semver.org/), namely that:

* incrementing the MAJOR version denotes incompatible API changes;
* incrementing the MINOR version denotes functionality added in a backward-compatible manner; and
* incrementing the PATCH version denotes bug fixes made in a backward-compatible manner.

If semantic versioning is used then:

* A MAJOR.0.0 version SHALL advertise all its MAJOR.MINOR.0 (MINOR ≠ 0) versions as usable without modification.
* A MAJOR.MINOR.0 version (MINOR ≠ 0) SHALL advertise the next MAJOR.MINOR’.0 (MINOR’ = MINOR + 1) version as usable without modification.
* A MAJOR.MINOR.0 version SHALL advertise all its MAJOR.MINOR.PATCH (PATCH ≠ 0) versions as usable without modification.
* A MAJOR.MINOR.PATCH version (PATCH ≠ 0) SHALL advertise the next MAJOR.MINOR.PATCH’ (PATCH’ = PATCH + 1) version as usable without modification.
* All MAJOR.MINOR.PATCH versions SHALL advertise the next MAJOR’.0.0 (MAJOR’ = MAJOR + 1) version as an upgrade, not guaranteed to be usable without modification.

These statements may be illustrated as follows:

![Semantic versioning](data:image/png;base64,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uOC5lb7Prqwuj11HmxzSJKD3E1+cPW7qYZeNYXWQAFaBBnqMleluVfcYvLb4C8bwyLdABs/5wZMODhTN8CgT4By4KANndSRAAiRAAiRAAiRQUwIUcDUlyPwkQAIkQAIkQAIkEGUCFHBRBs7qSIAESIAESIAESKCmBCjgakqQ+UmABEiABEiABEggygQo4KIMnNWRAAmQAAmQAAmQQE0JUMDVlCDzkwAJkAAJkAAJkECUCVDARRk4qyMBEiABEiABEiCBmhKggKspQeYnARIgARIgARIggSgToICLMvBIVPf1119LcXGxzJo1S7Zs2RKJKlgmCZAACZAACZCAgwhQwDnoYoTalJKSEunWrZucdNJJUl5eLo8++qi0b99e3nnnnVCLYnoSIAESIAESIIE4IsBHacXRxfJt6siRI6Vjx46yYsUKadKkiXz22WeyefNm6d27t/bIpaSk+GbhOQmQAAmQAAmQQB0g4DgBt3jxYj0UWAfYRrwL06dPl7Vr13rV07NnTznwwAPllltukYEDB3rFxfKkX79+0rp161g2gXVHgEBpQkdJqfS+ByNQTZ0sskyxSya7Onlt2SkSiAYBRwm4rKwsefLJJ2Xy5MnR6Hvc19GiRQvp0KGDXz8uueQSGTt2rDzyyCN+cbEIqKyslMMPP1y+/fbbWFRfL+pMSkiSWf+cFfW+tntpvSw9p600So3v2RgL/r0gJuw2XNA+6vWyQmcQyEjJkFjcd87oPVtRGwQcJeDS09Plm2++kf79+9dG3+ptGVdeeaXgzykGr+ro0aOd0hy2gwRIgARIgATinkB8/2yOe/zsAAmQAAmQAAmQAAmEToACLnRmzEECJEACJEACJEACMSVAARdT/KycBEiABEiABEiABEInQAEXOjPmIAESIAESIAESIIGYEqCAiyl+Vk4CJEACJEACJEACoROggAudGXOQAAmQAAmQAAmQQEwJUMDFFD8rJwESIAESIAESIIHQCVDAhc6MOUiABEiABEiABEggpgQo4GKKn5WTAAmQAAmQAAmQQOgEKOBCZ8YcJEACJEACJEACJBBTAhRwMcXPykmABEiABEiABEggdAIUcKEzYw4SIAESIAESIAESiCkBCriY4mflJEACJEACJEACJBA6AQq40JkxBwmQAAmQAAmQAAnElAAFXEzxs3ISIAESIAESIAESCJ0ABVzo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RheuCTFHzZO3e2m9/HBGG2nbgM9ErDlNllDfCDQevbe+dZn9JQFHEIjyt70j+lwnG1FUVikVsehZYkosaq31OksrKmu9TBZIAiRAAiRAApEiQAEXKbIslwRIgARIgARIgAQiRIACLkJgWSwJkAAJkAAJkAAJRIoABVykyLJcEiABEiABEiABEogQAQq4CIFlsSRAAiRAAiRAAiQQKQIUcJEiy3JJgARIgARIgARIIEIEKOAiBJbFkgAJkAAJkAAJkECkCFDARYosyyUBEiABEiABEiCBCBGggIsQWBZLAiRAAiRAAiRAApEiQAEXKbIslwRIgARIgARIgAQiRIACLkJgWSwJkAAJkAAJkAAJRIoABVykyLJcEiABEiABEiABEogQAQq4CIFlsSRAAiRAAiRAAiQQKQIUcJEiy3JJgARIgARIgARIIEIEKOAiBJbFkgAJkAAJkAAJkECkCFDARYosyyUBEiABEiABEiCBCBGggIsQWBZLAiRAAiRAAiRAApEiQAEXKbIslwRIgARIgARIgAQiRCA5QuWy2BgQmJdbLFkpCTGoOf6r/GJjkbRIT4r/jsSgB7PWFkpGMu+7cNC/tWZvONmYhwRIIMoEFuSVRLnG6qtLqFRWfTKmcDqBcfPy5dftzrvBnM4N7VtfUCalFfHQUue1sai8UvgBEt514SdveNyYiwRiRaBdg0SZN6JNrKr3q5cCzg8JA0iABEiABEiABEjA2QQ4B87Z14etIwESIAESIAESIAE/AhRwfkgYQAIkQAIkQAIkQALOJkAB5+zrw9aRAAmQAAmQAAmQgB8BCjg/JAwgARIgARIgARIgAWcToIBz9vVh60iABEiABEiABEjAjwAFnB8SBpAACZAACZAACZCAswlQwDn7+rB1JEACJEACJEACJOBHgALODwkDSIAESIAESIAESMDZBCjgnH192DoSIAESIAESIAES8CNAAeeHhAEkQAIkQAIkQAIk4GwCFHDOvj5sHQmQAAmQAAmQAAn4EaCA80PCABIgARIgARIgARJwNgEKOGdfH7aOBEiABEiABEiABPwIUMD5IWEACZAACZAACZAACTibAAWcs68PW0cCJEACJEACJEACfgQo4PyQMIAESIAESIAESIAEnE2AAs7Z14etIwESIAESIAESIAE/AhRwfkgYQAIkQAIkQAIkQALOJpAcqHn5Y86XwjemBYquU+GtluZKYouWtdan4a8Ol9yC3Forz8kFfTvqW0lLSqu1Ju6a2kkq926ptfKcXFDj0Xud3Dy2jQRIgARIwMEE6IFz8MVh00iABEiABEiABEjAjgAFnB0VhpEACZAACZAACZCAgwlQwDn44rBpJEACJEACJEACJGBHgALOjgrDSIAESIAEQiKQkJAQMD3iJk2aFDC+phGm7srKSvnhhx88xaWkpMiYMWM857V1UFpaKrt3766t4oIux/QTGXC8a9cunRf9vvvuu3XY999/r1+DLtQmIcr75ptvPDGo64477vCc88AZBCjgnHEd2AoSIAESIIEwCdx4440656effipHHnlkmKUEny01NVX+/PPP4DNEICX6nJbmWkA2evRoefzxxwVhvXv31q81qfKVV16RY445xlMEyh0wYIDnnAfOIEAB54zrwFaQAAmQAAmESWDChAlh5ozfbOizEXALFiyQrl27CsKysrL0a232DOWeeOKJtVkky7IQwLV7/fXXLSHBHVLABceJqUiABEigzhCwDsWhUzk5OXLqqafq/o0bN04PwSEN/u68805Pv4cMGeIVd/HFF3vicPDQQw954m+66SavOHOycuVKadCggSfdCSecIOXl5Sba8/rUU0952oTA//u//9N5TILff//d4xUy/UH7YOYcxzt27JDs7GxPfU888QSCtU2cONETjjxz5swxUTr8t99+85yvX79eh2VkZOiwPn36yKBBgzzx5uD555/X6c466yxP2V988YWce+65nvO77rpLJ8dQ5S233OIJRxvmzZvniYNoQhj+vvzyS1OFfkUYhlCnTp0qP/30k3z33Xc6XV5enn5ForKyMjnppJM8ZZx33nlSUVGh82NI25SNV/SrqKhIx51//vn6FeEwvJoh1HXr1nnxPOWUU3Q9SDd9+nSdtmfPnp6y4RFFP2mBCezdu1fOOeccyczMlLfffjtoXhRwgZkyhgRIgATqHYEHHnhACwN86eLL2nxxQygsW7ZMf8kjDoLmxRdf9OIDIYY4iIhHHnlENm/e7BWPk/79+8uMGTN0usLCQv1F/+CDD/qlGzVqlJdomTJlijRr1swjMm699Va5/PLLvfLNnj1bn1sFA0TZmjVrtHCZOXOm3HzzzTrNqlWr5IYbbtBthKhZuHChDB48WAs+r0J9TtBm2OLFiwXCLJCh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)

Furthermore, any MAJOR.MINOR.PATCH MAY advertise any later MAJOR.MINOR’.PATCH’ (MINOR’ = MINOR and PATCH’ > PATCH or MINOR’ > MINOR) version as usable without modification.

## Proposal

### Method Name Generation

To eliminate the possibility of method name overloading, names SHALL be set by some well-defined randomization algorithm. Some options are:

* random fixed-length string generation using a [cryptographically secure pseudorandom number generator](https://en.wikipedia.org/wiki/Cryptographically_secure_pseudorandom_number_generator);
* UUID generation per [RFC 9562](https://www.rfc-editor.org/rfc/rfc9562.html) (version 7 recommended), minus hyphens; and
* the hash of some document accessible to users of the DID method.

This satisfies the [Collision](#collision) requirement.

The last option could be implemented as the hash of the DID method specification. For the sake of this discussion, that option will be assumed. This satisfies the [Security](#security) requirement.

For future-proofing, the hash SHALL be represented using [multihash](https://github.com/multiformats/multihash), encoded in lowercase [Base36](https://en.wikipedia.org/wiki/Base36) to minimize the method name length.

If version control is not required, or if new versions will be advertised out-of-band (e.g., through a public or private messaging service), no further work needs to be done.

### Version Advertisement

For a DID method to be able to advertise new versions of itself, there SHALL be a way to query it, in a way that is accessible to all parties authorized to use the DID method, either in DID generation or in DID verification. If there are restrictions around the use of a DID (e.g., access to the DID method’s verifiable data registry requires presentation of some authorization token), those same restrictions MAY apply to querying the DID method.

Supporting version advertisement requires standardization of the advertisement attributes. The recommended top-level attributes are:

| Attribute         | Type     | Definition                                                     |
|-------------------|----------|----------------------------------------------------------------|
| method            | Method   | Method object defining the DID method. Required.               |
| compatibleMethods | Method[] | Array of Method objects defining compatible methods. Optional. |
| replacementMethod | Method   | Method object defining the replacement method. Optional.       |

The recommended Method object attributes are:

| Attribute     | Type      | Definition                                                                           |
|---------------|-----------|--------------------------------------------------------------------------------------|
| id            | URI       | The URI representing the DID method. Required.                                       |
| version       | string    | The semantic version represented by the DID method name. Optional.                   |
| published     | timestamp | The date and time, in XML Datetime format, when the version was published. Optional. |
| specification | URI       | The URI representing the DID method specification. Optional.                         |

Without having to invent any new standard, there are three options for the DID method to provide this data. All three depend on the DID method having its own ID, and it is proposed that the ID be a DID URI within the same DID method. This has the advantage that any client capable of interacting with DIDs within the DID method will also be capable of interacting with the DID identifying the DID method.

The three options for the DID method to provide this data are:

* within the DID document data;
* within the DID document metadata; or
* via a service associated with the DID.

Providing the data within the DID document data is likely the easiest for client applications, but it depends on the DID document being updatable, which is not always the case. DID document metadata has a similar issue with updatability. Using a service provides the most flexibility as the mutable nature of the data better aligns with the concept of a service: data returned by a service is expected to vary based on implicit parameters such as the date and time of the request. Furthermore, the "Version” (proposed) service type can be included in every DID document for the method, allowing the query to take place using any DID for the DID method.

### Implementation

Because the method name is the hash of the method specification, it’s not possible to include the method name in the specification itself. Instead, the specification SHALL include the following statement:

> The method name for this specification is the _&lt;hash algorithm name>_ hash of this document, represented using [multihash](https://github.com/multiformats/multihash), encoded in lowercase [Base36](https://en.wikipedia.org/wiki/Base36).

For the DID representing the DID method to be known, the specification MAY include the following statement:

> The DID representing the DID method for this specification is <code>did:<em>method-name</em>:<em>&lt;method-specific-id></em></code>.

Depending on the algorithm for generating the method-specific ID, it may not be possible to know it at the time that the specification is finalized, so it may instead be shared through some out-of-band mechanism.

### Standardization

There are two paths to standardization.

The preferred option is to update the DID standard to support self-describing DID methods. The DID working group has reconvened, and it’s a good opportunity to put this forward as a requirement, assuming that it’s in scope of the current charter.

The alternative is to register a new DID method (e.g., “x”), to support self-describing DID methods as sub-methods. This would include registration of the additional attributes and the "Version” service type.

## Scenarios

These scenarios assume that the self-describing DID method is at the top level (i.e., not subordinate to another DID method such as “x”) and that version information is published through a service endpoint.

### DID Method Development

Legendary Requirements proposes a bitcoin-based DID method. Rather than reserve a bitcoin-like method name (e.g., “btclr”) for it, they opt to make it self-describing.

DIDs for this method use a bitcoin address as the method-specific identifier, and one is reserved specifically for the method. The specification contains the following statements:

> The method name for this specification is the SHA-256 hash of this document, represented using [multihash](https://github.com/multiformats/multihash), encoded in lowercase [Base36](https://en.wikipedia.org/wiki/Base36).
>
> The DID representing the DID method for this specification is <code>did:<em>method-name</em>:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2</code>.

Once finalized, the hash is calculated and the final DID for the method is determined to be `did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2`. The method name and DID are published to the (private or public) community, with the DID document for the method appearing as follows:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v2",
    "..."
  ],
  "id": "did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
  "authentication": [
    "..."
  ],
  "service": [
    {
      "id": "did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2#version-1",
      "type": "Version",
      "serviceEndpoint": "ipns://QmRnywyciVGEKdd1sYR6rpiF27DPRkKBdKkYNBaPtdCmDN"
    }
  ]
}
```

The DID document contains an authentication method and a “Version” service endpoint. The endpoint uses the InterPlanetary Naming System protocol to locate the version file, making it persistent, updatable, and discoverable. The initial file contains the following:

```json
{
  "method": {
    "id": "did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.0.0",
    "published": "2023-01-27T15:00:00Z",
    "specification": "ipfs://QmXApLFURjcNLiBJNEFMci7gAeoUnx411RLBhRjzHWiq9f"
  }
}
```

### Patch Version

After release, users note some inconsistencies in the specification. Legendary Requirements updates the specification with some errata, none of which have an effect on previously-created DIDs. The new version is published with a new DID method name, with the DID document for the method appearing as follows:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v2",
    "..."
  ],
  "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
  "authentication": [
    "..."
  ],
  "service": [
    {
      "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2#version-1",
      "type": "Version",
      "serviceEndpoint": "ipns://Qmb8udPuUo9ckGUQ2uMSktaHqQ6FFdSNK3aoFZjgj6P1xr"
    }
  ]
}
```

The DID document is much the same as before, with the updated DID method name and an updated “Version” service endpoint. Querying the service endpoint for the original version now returns the following:

```json
{
  "method": {
    "id": "did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.0.0",
    "published": "2023-01-27T15:00:00Z",
    "specification": "ipfs://QmXApLFURjcNLiBJNEFMci7gAeoUnx411RLBhRjzHWiq9f"
  },
  "compatibleMethods": [
    {
      "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
      "version": "1.0.1",
      "published": "2023-02-17T03:00:00Z",
      "specification": "ipfs://QmVqJyXNPf25tnsXAQkPtviYvCtkAWK5UZHqWuUF9g8onY"
    }
  ]
}
```

Querying the service endpoint for the patch version returns the following:

```json
{
  "method": {
    "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.0.1",
    "published": "2023-02-17T03:00:00Z",
    "specification": "ipfs://QmVqJyXNPf25tnsXAQkPtviYvCtkAWK5UZHqWuUF9g8onY"
  }
}
```

Note that this is identical to the first compatible method for the original version, as expected.

### DID Creation

A developer creates a wallet app that uses the version 1.0.0 DID method created by Legendary Requirements. As DID wallet is approaching production deployment, the developer checks for any updates and discovers patch version 1.0.1. She reviews the specification and determines that there is nothing that would affect her code, so she updates the DID method name, reruns the tests, and deploys the wallet app.

### Minor Version

Legendary Requirements updates the specification to add a new feature without breaking existing functionality. The new version is published with a new DID method name, with the DID document for the method appearing as follows:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v2",
    "..."
  ],
  "id": "did:muhkjpec6ivnuxbyw9hu0ffoahm5n0wvoq0vq4n2zx3ekyvz6ghf:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
  "authentication": [
    "..."
  ],
  "service": [
    {
      "id": "did:muhkjpec6ivnuxbyw9hu0ffoahm5n0wvoq0vq4n2zx3ekyvz6ghf:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2#version-1",
      "type": "Version",
      "serviceEndpoint": "ipns://Qmaafp2Kx6vaqG7LMXoJzoQjpi9RJxBddvfuW764KvuRdq"
    }
  ]
}
```

Querying the service endpoint for the original version now returns the following:

```json
{
  "method": {
    "id": "did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.0.0",
    "published": "2023-01-27T15:00:00Z",
    "specification": "ipfs://QmXApLFURjcNLiBJNEFMci7gAeoUnx411RLBhRjzHWiq9f"
  },
  "compatibleMethods": [
    {
      "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
      "version": "1.0.1",
      "published": "2023-02-17T03:00:00Z",
      "specification": "ipfs://QmVqJyXNPf25tnsXAQkPtviYvCtkAWK5UZHqWuUF9g8onY"
    },
    {
      "id": "did:muhkjpec6ivnuxbyw9hu0ffoahm5n0wvoq0vq4n2zx3ekyvz6ghf:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
      "version": "1.1.0",
      "published": "2023-09-10T20:00:00Z",
      "specification": "ipfs://QmYHh1AWSNVerTmpm5qyN8hNf8cQWoHLCCuSX2VuNzbbit"
    }
  ]
}
```

Querying the service endpoint for the patch version returns the following:

```json
{
  "method": {
    "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.0.1",
    "published": "2023-02-17T03:00:00Z",
    "specification": "ipfs://QmVqJyXNPf25tnsXAQkPtviYvCtkAWK5UZHqWuUF9g8onY"
  },
  "compatibleMethods": [
    {
      "id": "did:muhkjpec6ivnuxbyw9hu0ffoahm5n0wvoq0vq4n2zx3ekyvz6ghf:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
      "version": "1.1.0",
      "published": "2023-09-10T20:00:00Z",
      "specification": "ipfs://QmYHh1AWSNVerTmpm5qyN8hNf8cQWoHLCCuSX2VuNzbbit"
    }
  ]
}
```

Querying the service endpoint for the minor version returns the following:

```json
{
  "method": {
    "id": "did:muhkjpec6ivnuxbyw9hu0ffoahm5n0wvoq0vq4n2zx3ekyvz6ghf:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.1.0",
    "published": "2023-09-10T20:00:00Z",
    "specification": "ipfs://QmYHh1AWSNVerTmpm5qyN8hNf8cQWoHLCCuSX2VuNzbbit"
  }
}
```

### Minor Version Presentation

Verifier software that supports Legendary Requirements’s bitcoin-based DID method (among others) is given a verifiable presentation with an unfamiliar DID method. Verification is suspended while the verifier checks the “Version” service endpoints of all supported self-describing DID methods and discovers that the unknown DID method is for version 1.1.0 of the Legendary Requirements’s bitcoin-based DID method.

Because the software understands how to interact with version 1.0.0, it can support the new DID method (though not its new features), so it adds the DID method to the table and continues with the presentation verification. Optionally, it sends a notification to the developers indicating that a new minor release is available so that they may investigate it.

### Major Version

Legendary Requirements reworks the DID method to take advantage of new features in the Bitcoin lighting protocol. The new version is incompatible with prior versions and so is given a new major version number. The new version is published with a new DID method name and bitcoin, with the DID document for the method appearing as follows:

```json
{
  "@context": [
    "https://www.w3.org/ns/did/v2",
    "..."
  ],
  "id": "did:mufcbql5ys1tk9vm942tsihusqso9nkcf3tyxtrrv1xkekap18ck:bc1q5nml2l4eu92za637c6st8t9fus37xrl0a4au20",
  "authentication": [
    "..."
  ],
  "service": [
    {
      "id": "did:mufcbql5ys1tk9vm942tsihusqso9nkcf3tyxtrrv1xkekap18ck:bc1q5nml2l4eu92za637c6st8t9fus37xrl0a4au20#version-1",
      "type": "Version",
      "serviceEndpoint": "ipns://QmTnmu8pNoAAijwarFY3zwi6UQ2b726tniyTHkYXjDK2Eq"
    }
  ]
}
```

Querying the service endpoint for the original version now returns the following:

```json
{
  "method": {
    "id": "did:mugx0x8lmmlu9m5ysvjrq222spgz8aovnrgbco63zzuutdztwbsu:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
    "version": "1.0.0",
    "published": "2023-01-27T15:00:00Z",
    "specification": "ipfs://QmXApLFURjcNLiBJNEFMci7gAeoUnx411RLBhRjzHWiq9f"
  },
  "compatibleMethods": [
    {
      "id": "did:muiilbl2232lh8pbbn8zcm4s8eiarsg5ahd2tvxqj3r9pfj7zv5t:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
      "version": "1.0.1",
      "published": "2023-02-17T03:00:00Z",
      "specification": "ipfs://QmVqJyXNPf25tnsXAQkPtviYvCtkAWK5UZHqWuUF9g8onY"
    },
    {
      "id": "did:muhkjpec6ivnuxbyw9hu0ffoahm5n0wvoq0vq4n2zx3ekyvz6ghf:bc1qsvqcrsqhzmsfrn45jlsctsfmjw7yeq38dvn9v2",
      "version": "1.1.0",
      "published": "2023-09-10T20:00:00Z",
      "specification": "ipfs://QmYHh1AWSNVerTmpm5qyN8hNf8cQWoHLCCuSX2VuNzbbit"
    }
  ],
  "replacementMethod": {
    "id": "did:mufcbql5ys1tk9vm942tsihusqso9nkcf3tyxtrrv1xkekap18ck:bc1q5nml2l4eu92za637c6st8t9fus37xrl0a4au20",
    "version": "2.0.0",
    "published": "2024-05-27T22:00:00Z",
    "specification": "ipfs://QmPYtDfWv9YmSPGgQDY3SDu6qLqfSu8jbbqEh23hMtvV36"
  }
}
```

Similarly, patch version 1.0.1 and minor version 1.1.0 have the `replacementMethod` attribute added to their version files.

Querying the service endpoint for the new major version returns the following:

```json
{
  "method": {
    "id": "did:mufcbql5ys1tk9vm942tsihusqso9nkcf3tyxtrrv1xkekap18ck:bc1q5nml2l4eu92za637c6st8t9fus37xrl0a4au20",
    "version": "2.0.0",
    "published": "2024-05-27T22:00:00Z",
    "specification": "ipfs://QmPYtDfWv9YmSPGgQDY3SDu6qLqfSu8jbbqEh23hMtvV36"
  }
}
```

### Major Version Presentation

Verifier software that supports Legendary Requirements’s bitcoin-based DID method (among others) is given a verifiable presentation with an unfamiliar DID method. Verification is suspended while the verifier checks the “Version” service endpoints of all supported self-describing DID methods and discovers that the unknown DID method is for version 2.0.0 of the Legendary Requirements’s bitcoin-based DID method.

Because the DID method is a replacement, the software doesn’t understand how to interact with it, so it rejects the presentation and sends a notification to the developers indicating that a new major release is available so that they may investigate it.

## Security Considerations

TBD.
