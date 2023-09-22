
# Automated Peering API

Initial proof of concept for Automated Peering API

See [CONTRIBUTING.md](CONTRIBUTING.md)

```mermaid
sequenceDiagram
    participant Meta
    participant PeeringDB
    participant Peer
    Peer->>Meta: Request URL for login
    Meta->>Peer: Sends back URL
    Peer->>PeeringDB: Sends request for auth code
    PeeringDB->>Peer: Replies back with auth code
    Peer->>Meta: Sends auth code to Meta
    Meta->>PeeringDB: Exchange auth code for token
    PeeringDB->>Meta: Send token back to Meta
    Note left of Meta: Now Meta knows what peer can do
    Meta->>Peer: Send OK back to peer
    Peer->>Meta: QUERY peering locations (peer type, ASN, auth code)
    Meta->>Peer: Meta replies back<br/>200 (peering locations, request ID)<br/>401 Not Authorized<br/>406 Invalid Locations<br/>451 Denied<br/>Additional JSON with info on sessions
    Peer->>Meta: If 200, peer sends QUERY request status (request ID, auth code)
    Meta->>Peer: Meta replies with responses:<br/>200 Configured (peer asn, fids, list of sessions)<br/>404 bad request ID<br/>202 waiting<br/>20x waiting for peer to configure<br/>Additional JSON with info on sessions
    loop until both sides report peering complete
        Peer->>Meta: peer sends QUERY request status
		Meta->>Peer: Meta replies with responses:<br/>200 Configured (peer asn, fids, list of sessions)<br/>404 bad request ID<br/>202 waiting<br/>20x waiting for peer to configure<br/>Additional JSON with info on sessions
    end
```

## License

By contributing to this repo, you agree that your contributions will be
licensed under the LICENSE file in the root directory of this source tree.

Documentation is covered by the Creative Commons Attribution 4.0 (CC-BY-4.0)
license.
