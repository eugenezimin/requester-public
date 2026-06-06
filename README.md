# Requester

### Native HTTP load testing for macOS — fire configurable traffic at any endpoint and watch it respond in real time.

![[main_window.png]]

## What is Requester?

Requester is a lightweight, fully native macOS app for putting HTTP endpoints under load. Point it at a URL, set how hard you want to push, and hit **Start**. Requests fire at your target rate while a live chart plots throughput, successes, failures, and error rate second by second. No browser tab, no YAML config, no Docker — just a clean desktop tool that does one thing well.

Built entirely in **Swift and SwiftUI** with structured concurrency under the hood, Requester runs as a real macOS app, not a cross-platform wrapper.

## Why Requester?

- **Real-time visibility.** A live Swift Charts graph shows sent vs. received throughput, failures, and error rate as the test runs — not a summary report after the fact.
- **Honest backpressure.** When your target is faster than the endpoint can answer, Requester _shows_ the achieved rate dropping instead of silently buffering forever. That dip is the signal you actually want.
- **Inspect every exchange.** A built-in request/response viewer lets you see exactly what's going over the wire, with sliding-window history and optional truncation.
- **Native feel.** Keyboard navigation, proper macOS panels, header autocomplete, and a real menu-bar About window. It behaves like a Mac app because it is one.

## Key Features

### 🎯 Dashboard — your run at a glance

Everything that defines a run, summarized in one card: transport, RPS-per-channel and channel count, stop conditions, active headers, backpressure queue depth, User-Agent preset, and the computed total target RPS.

![[dashboard.png]]

### ⚡ Load configuration — dial in exactly how hard to push

Throughput is modeled as **RPS per channel × channels**, so the total target rate is explicit and predictable. Set your channel count, per-channel rate, stop conditions (total requests _or_ duration), backpressure queue depth, and pick a User-Agent from realistic browser presets.

![[load_tab.png]]

### 📈 Live chart — four series, fully interactive

The chart plots **Sent**, **Received**, **Failures**, and **Error Rate** as separate series. Tap any legend pill to toggle a series on or off. Drag the chart to pan back through time, and set the visible window from 5 seconds up to 5 minutes.

![[chart.png]]

### 📊 Stats — the numbers that matter

A clean grid of live stat cards: elapsed time, sent, succeeded, failed, current RPS, average RPS, average latency, backpressure queue depth, and your configured target.

![[stats.png]]

### 🔍 Request & Response inspector

See the exact request being fired and inspect live responses as they come back. Toggle **Sliding** mode to keep a rolling window of the most recent responses, cap how many you retain, and flip on **Truncated** to collapse long bodies.

![[texts.png]]

### 🧩 Header editor with autocomplete

Add, edit, and remove request headers in a scrollable editor. Header-name fields autocomplete against the standard HTTP request header set as you type.

![[headers.png]]

### 🎛️ Full control, always one click away

Start, pause, and reset live from the bottom bar, with elapsed time and total sent count always in view. Validation errors surface inline before a run ever starts.

## How it works under the hood

Requester is built on Swift structured concurrency end to end:

- A **dispatcher** fires requests at your target interval without ever blocking on a response — each request runs as an independent child task.
- An **actor-based in-flight counter** enforces your channel limit. When every channel is busy, new requests wait; when the wait queue is full, excess requests are **shed** rather than buffered indefinitely.
- Per-request outcomes flow into an isolated counter actor, and a snapshot timer reads it each sampling interval to drive the live chart and stats.

The result: when an endpoint can't keep up, you see the achieved RPS honestly drop below target instead of getting a misleading "everything's fine" reading.

## Roadmap

HTTP load testing ships today. The architecture is already protocol-agnostic, with these transports on the way:

- **gRPC** endpoint testing
- **WebSocket** load
- **Raw TCP** load

## Requirements

- **macOS 14** or later
- Apple Silicon or Intel Mac

## Getting started

1. Download and open **Requester**.
2. Enter a target URL.
3. Set your channels and RPS per channel on the **Load** tab.
4. Choose a stop condition — total requests, a duration, or both.
5. Hit **Start** and watch the chart.

## About

Requester is created by **Eugene Zimin**. 🔗 [github.com/eugenezimin](https://github.com/eugenezimin)

<div align="center">

© 2026 Eugene Zimin. All rights reserved.

</div>
