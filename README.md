# tflite-c-binaries

Prebuilt TensorFlow Lite C `.xcframework` archives, hosted so a Swift package can depend on them as a
`.binaryTarget(url:checksum:)`. Nothing here is Knowra's own code: each release carries the upstream
TensorFlow Lite C framework for iOS, repackaged in the shape Swift Package Manager expects (the
`.xcframework` at the ZIP's root) and code-signed, under the upstream Apache-2.0 licence (`LICENSE`).

## Releases

| Tag | Upstream | Archive | `swift package compute-checksum` |
|---|---|---|---|
| `tflite-c-2.17.0` | TensorFlowLiteC 2.17.0 (`TensorFlowLiteC-2.17.0.tar.gz`, sha256 `9667b476015f136e5b332ce040e12822c4ac6d5c58947882ddc809cdff0fb99e`, 80,242,958 bytes) | `TensorFlowLiteC.xcframework.zip`, 34,533,145 bytes | `a3973e2581666aedf3f5f2a76f533c6463f33aa4d1aa815b79f04841bc3bebfe` |

A tag's asset is never replaced: a re-cut of an already-tagged version is a new tag `tflite-c-<version>-<n>` with its own checksum, so a manifest that pinned the earlier tag keeps verifying the bytes it was written against.

Only the interpreter framework (`TensorFlowLiteC`) is included — not `TensorFlowLiteCMetal` or
`TensorFlowLiteCCoreML`. Slices: `ios-arm64` and `ios-arm64_x86_64-simulator`; no macOS slice.

## How an archive is produced

By the consuming SDK's provisioning script from the pinned upstream tarball: the tarball's sha256 and
byte count are both asserted before extraction, the framework is re-signed (upstream ships it
unsigned), an `Info.plist` is written for the embedded framework, and the archive is zipped with the
`.xcframework` at its root. A re-run re-signs and the archive checksum changes, so the checksum above
belongs to the one archive attached to the release and is never recomputed from a later run.

## Using it

```swift
.binaryTarget(
    name: "TensorFlowLiteC",
    url: "https://github.com/knowra-id/tflite-c-binaries/releases/download/tflite-c-2.17.0/TensorFlowLiteC.xcframework.zip",
    checksum: "a3973e2581666aedf3f5f2a76f533c6463f33aa4d1aa815b79f04841bc3bebfe")
```

## Licence

Apache License 2.0, as upstream (`LICENSE`, the text at `tensorflow/tensorflow` tag `v2.17.0`). See
`NOTICE` for the upstream source.
