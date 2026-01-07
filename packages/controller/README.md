# Scramjet Controller

The controller package manages the coordination between the main application, service workers, and proxy transports in the Scramjet proxy. It handles RPC communication, request routing, and resource management across different execution contexts.

## Overview

This package provides the core orchestration layer for Scramjet, enabling:

- **RPC Communication**: Two-way RPC communication between the main thread, service workers, and transport layers
- **Request Routing**: Handling and forwarding HTTP/HTTPS requests through configured proxy transports
- **Resource Injection**: Injecting Scramjet utilities into web pages
- **Service Worker Integration**: Managing multiple service worker instances and their lifecycle
- **Cookie Management**: Centralized cookie jar for maintaining session state across requests

## Structure

### Core Modules

- **`index.ts`** - Main Controller class that coordinates between transports and service workers. Handles request processing, frame management, and RPC setup.
- **`inject.ts`** - Injection script that runs in the service worker context. Implements RemoteTransport to communicate back to the main controller via MessagePort.
- **`sw.ts`** - Service worker hooks that manage controller registration, request interception, and service worker lifecycle.
- **`types.d.ts`** - Type definitions for RPC messages and communication interfaces between different contexts.
- **`typesEntry.ts`** - Type entry point for the package exports.

## Exports

The package exports three main entry points:

- **`@petezah-games/scramjet-controller`** - Main Controller API
- **`@petezah-games/scramjet-controller/inject`** - Injection utilities for service workers
- **`@petezah-games/scramjet-controller/worker`** - Service worker

## Key Features

### Controller Class

The main `Controller` class manages:
- Connection to proxy transports
- Service worker lifecycle and registration
- Frame management for multiple contexts
- RPC communication with service workers
- Request interception and routing via Scramjet

### Configuration

Default configuration includes:
- `prefix`: `/~/sj/` - URL prefix for controller routes
- `virtualWasmPath`: `scramjet.wasm.js` - Virtual path for WASM module
- `injectPath`: `/controller/controller.inject.js` - Path to injection script
- `scramjetPath`: `/scramjet/scramjet.js` - Path to main Scramjet library
- `wasmPath`: `/scramjet/scramjet.wasm` - Path to WASM binary

### RPC Methods

The controller exposes RPC methods for:
- `ready` - Signals when the service worker is ready
- `request` - Processes HTTP requests
- Additional transport initialization and management

## Dependencies

- `@mercuryworkshop/scramjet` - Core proxy rewriting library
- `@mercuryworkshop/proxy-transports` - Transport abstraction layer
- `@mercuryworkshop/rpc` - RPC helper for inter-context communication

## Usage

The controller is typically initialized by the bootstrap package, which sets up the service worker registration and injects the controller into the page context. Applications should interact with the controller through the RPC interface for request handling and configuration.
