
# Automated Peering API

Initial proof of concept for Automated Peering API

See [CONTRIBUTING.md](CONTRIBUTING.md)

```mermaid
sequenceDiagram
    participant Initiator
    participant Peer
    participant PeeringDB

    Initiator->>PeeringDB: OIDC Authentication
    PeeringDB->>Initiator: Provide auth code
    Initiator->>Peer: Send auth code to Peer
    Peer->>PeeringDB: Exchange auth code for token
    PeeringDB->>Peer: Return token
    Note left of Peer: Peer determines permissions based on token
    Peer->>Initiator: Send OK back to Initiator

    Initiator->>Peer: QUERY peering locations (peer type, ASN, auth code)
    Peer->>Initiator: Reply with peering locations or errors (401, 406, 451, etc.)

    alt 200 response from Peer
        Initiator->>Peer: QUERY request status using request ID & auth code
        Peer->>Initiator: Reply with session status (200, 404, 202, etc.)
        loop until peering is complete
            Initiator->>Peer: QUERY request status
		    Peer->>Initiator: Session status
        end
    end
```

## License

By contributing to this repo, you agree that your contributions will be
licensed under the LICENSE file in the root directory of this source tree.

Documentation is covered by the Creative Commons Attribution 4.0 (CC-BY-4.0)
license.
