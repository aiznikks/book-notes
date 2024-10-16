#!/bin/bash

# Root directory (you can modify this to specify a different directory)
root_dir="."

# Function to process each directory
process_directory() {
    local dir=$1

    # Check if there is any .onnx file in the directory
    if ls "$dir"/*.onnx &> /dev/null; then
        echo "Found .onnx file in $dir"
        
        # Check if config.cfg is present in the same directory
        if [ -f "$dir/config.cfg" ]; then
            echo "Found config.cfg in $dir"
            
            # Replace occurrences in config.cfg
            sed -i 's/one-import-tflite/one-import-onnx/g' "$dir/config.cfg"
            sed -i 's/\.tflite/\.onnx/g' "$dir/config.cfg"
            
            echo "Updated config.cfg in $dir"
        else
            echo "config.cfg not found in $dir"
        fi
    fi
}

# Export the function to make it available for subshells (if needed)
export -f process_directory

# Recursively search for subdirectories and process them
find "$root_dir" -type d -exec bash -c 'process_directory "$0"' {} \;

echo "Script execution completed."
