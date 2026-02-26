# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0

# Multi-stage build: compile a custom OTel Collector that includes the
# subtraceaggregator processor for service-level trace insights.

FROM golang:1.24 AS builder

# Install the OpenTelemetry Collector Builder (ocb)
RUN CGO_ENABLED=0 go install go.opentelemetry.io/collector/cmd/builder@v0.145.0

WORKDIR /build

# Copy the builder manifest and processor source
COPY builder.yaml .
COPY subtraceaggregator/ /build/processor/subtraceaggregator/

# Build the custom collector binary
RUN builder --config=/build/builder.yaml --skip-compilation=false

FROM gcr.io/distroless/static:nonroot

COPY --from=builder /build/_build/otelcol-custom /otelcol

ENTRYPOINT ["/otelcol"]
