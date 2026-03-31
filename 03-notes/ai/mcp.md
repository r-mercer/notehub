# Model Context Protocol (MCP)

MCP is an open source standard for connecting AI functions to external systems. It is effectively the interface layer between AI models and the wider world, icluding data sources and other apps. 

This might include agents accessing calendars, generating FIGMA designs, or AI models creating a design in blender and printing it using a 3d printer.

## Architecture Overview

There are two participants in an MCP set up, the host and the server. The host is an AI app and the server is the well server that runs a thing, then there's also the client but I am not totally sure how that's different. 

From there, there are two layers, the data layer being the JSON-RPC based protocol for client server communication and then the transport layer, which defines the communication mechanism and channel that enable data exchnage betweeen server and clients. Including all hte boring shit you would expect on that front.
