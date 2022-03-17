# Yak Stack for Rovers

Yak Rovers act as a collective. Rovers, humans and non-roving applications interact using Web APIs.

## Connectivity

It's hard to predict the communication infrastructure that could be available
for Mars missions. To enable building for today we priorize common simple
mechanisms for communicating between Rovers and applications.

Each Rover is expected to have an IP address. Services are offered over HTTP.

Networks will be presumed secure. This is accomplished either physically (such
as a 140 million mile airgap to Mars) or via VPNs routed over the public
internet.

## VPN

On Earth, Rovers communicate over secured VPN powered by
[Tailscale](https://tailscale.com) (0-configuration WireGuard) which is offered
for free for personal use as well as for Open Source GitHub communities.

## API

The HTTP-based API for interacting with Rovers should be expected to evolve over time. 

Most interactions should be done with `Content-Type` of `json`.

To start Rovers should provide at least:

### GET /v1

Top level resource should respond with identifying information for the rover as
well as links to resources.

### POST /v1/CI

`CI` is "Command Injection" ([using NASA/JPL terminology](https://github.com/nasa/CFS_CI)).

The request is posted as a form with the keys:

* `cmd` - a text command

All other fields are `cmd` specific

The Rover is expected to respond with a `200` response on successful acceptance of the command.

### GET /v1/CAM[1-9]

The request retrieves still images from the identified camera. Response will be
in a binary image, such as `image/jpeg`.

###  GET /metrics

This provides telemetry formatted for
[prometheus](https://prometheus.io/docs/prometheus/latest/getting_started/)
consumption.

## Non-web/VPN capable Nodes

Some nodes may have restrictive compute or communication capabilities that do not allow it to fully implement this stack.

It is expected in this case that a "Ground Station" be provided to provide such capabilities to a Rover. 
As an example, see [Stubborn](https://github.com/rhettg/Stubborn#ground)