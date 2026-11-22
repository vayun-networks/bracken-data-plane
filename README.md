# BRACKEN Data Plane

**Fast packet handling for BRACKEN network nodes.**

The BRACKEN Data Plane is intended to process ingress and egress traffic on a node, apply configured forwarding and filtering behavior, and expose useful flow and performance signals to the wider platform. It is the packet-processing layer, separate from the management systems that configure it.

> **Project status:** Early-stage project. No throughput, latency, or hardware support guarantees are currently established.

## Responsibilities

- Receive and transmit packets through supported host interfaces.
- Classify packets and flows against installed policy.
- Apply forwarding, filtering, and traffic-shaping actions.
- Track flow state where required by enabled features.
- Support a staged exploration of Linux networking options such as XDP, eBPF, and AF_XDP; additional acceleration backends may be considered after measurement.
- Export counters and health information without placing control or analytics work in the packet hot path.

## Architecture boundary

The data plane executes the policy and forwarding state supplied by the [control plane](https://github.com/vayun-networks/bracken-control-plane). It should not own the source of truth for fleet configuration. Routing configuration and protocol integration belong with the [network engine](https://github.com/vayun-networks/bracken-network-engine); traffic analysis and reporting belong with [analytics](https://github.com/vayun-networks/bracken-analytics); attack classification and mitigation policy belong with [DDoS defense](https://github.com/vayun-networks/bracken-ddos).

## Engineering priorities

Correctness and safe failure behavior come before peak throughput. The design should make packet actions observable, policy updates bounded and reviewable, and resource use explicit. Performance work must include repeatable test conditions and report packet sizes, traffic shape, hardware, CPU configuration, and latency methodology.

## Planned milestones

1. Define supported packet paths and an initial backend.
2. Implement a minimal forwarding/filtering path with unit and integration coverage.
3. Add configuration and status interfaces.
4. Measure throughput, packet rate, CPU use, and tail latency in an isolated lab.
5. Evaluate additional acceleration only where benchmarks justify the complexity.

## Development

Build instructions, kernel requirements, capabilities, and lab setup will be documented when the initial implementation is selected. Do not run high-volume or attack-like traffic against third-party systems. Use isolated test environments and authorized synthetic traffic.

## Related components

- [bracken-control-plane](https://github.com/vayun-networks/bracken-control-plane)
- [bracken-network-engine](https://github.com/vayun-networks/bracken-network-engine)
- [bracken-ddos](https://github.com/vayun-networks/bracken-ddos)
- [bracken-observability](https://github.com/vayun-networks/bracken-observability)

## Security

The data plane may enforce security-sensitive network policy. Changes to parsing, filtering, memory handling, privileges, or update behavior should receive careful review and include relevant failure-mode analysis.