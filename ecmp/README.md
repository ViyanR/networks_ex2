# Simple Equal Cost Multi Path (ECMP)

In this exercise you will write your own control and dataplane programs that run on the switch model. Specifically, you will extend the previous forwarding exercise with support for a basic ECMP functionality. As identified the previous section, given the existing forwarding rules traffic from `h1` to `h4` follows a fixed path. The goal of this exercise is to decide per connection which path to take in case there are multiple paths. This per connection decision will happen by generating a hash of the 5-tuple (source IP, source port, destination IP, destination port, and L4 protocol) and deciding the next hop based on this hash value.

## Instructions

You are given skeleton files for the dataplane (`ecmp.p4`) and the control plane (`mycontroller.py`) which you need to extend.

Steps:
1. Extend the dataplane parser to parse the TCP header in the case of a TCP packet.
2. Create a metadata field to hold the ECMP hash value. The size of this field is determined by the number of alternative paths.
3. Create an action that computes the hash value for TCP packets. You can find the documentation for this `extern` functionality in the switch model implementation [here](https://github.com/p4lang/p4c/blob/main/p4include/v1model.p4#L444).
4. Create a table for the forwarding logic. You can extend the `ipv4_exact` table from the previous exercise. Note that you have to apply the ECMP forwarding logic only in the case of TCP traffic.
5. Implement the controller that populates the equivalent dataplane tables.

## Deliverables

To exercise the newly implemented functionality, you will use Mininet's `iperf` tool. This tool creates a TCP connection and generates TCP traffic between two hosts. Given that the client port selection is random, if implemented correctly, subsequent executions of `iperf h1 h4` should choose different paths.

In your report include screenshots of different `tcpdump` captures on the `s1-eth3` and `s1-eth4` interfaces that show the selected path for each execution. In your implementation, does the bidirectional TCP traffic select a symmetric path, or the transmit and receive traffic follow different paths?

You should also submit the `ecmp.p4` and `mycontroller.py` files.
