#!/bin/bash

# Set the target directory to the current directory
TARGET_DIR="$(pwd)"

echo "Cleaning up all files except .onnx and .tflite in: $TARGET_DIR"

# Find and remove all files except .onnx and .tflite in subdirectories
find "$TARGET_DIR" -type f ! \( -name "*.onnx" -o -name "*.tflite" \) -exec rm -f {} \;

echo "Cleanup completed."
