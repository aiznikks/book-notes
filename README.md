#!/bin/bash

# Traverse all subdirectories
for dir in $(find . -type d); do
    # Check if .cfg file exists in the directory
    cfg_file=$(find "$dir" -maxdepth 1 -name "*.cfg" -print -quit)
    if [[ -n "$cfg_file" ]]; then
        # Check if .onnx file exists in the same directory
        onnx_file=$(find "$dir" -maxdepth 1 -name "*.onnx" -print -quit)
        if [[ -n "$onnx_file" ]]; then
            # Get the prefix of the .onnx file (filename without extension)
            prefix=$(basename "$onnx_file" .onnx)
            echo "Processing directory: $dir"
            echo "Replacing 'OCTnet_v1' with '$prefix' in $cfg_file"
            # Replace all occurrences of 'OCTnet_v1' with the prefix in the .cfg file
            sed -i "s/OCTnet_v1/$prefix/g" "$cfg_file"
        fi
    fi
done
