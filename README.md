#!/bin/bash

# Loop through each directory in the current folder
for dir in */; do
    # Check if it's a directory
    if [ -d "$dir" ]; then
        # Check if there are any .tflite files in the directory
        if ls "$dir"*.tflite 1> /dev/null 2>&1; then
            echo "Processing directory: $dir"

            # Define the path to config.cfg
            config_file="$dir/config.cfg"
            
            # Check if config.cfg exists in the folder
            if [ -f "$config_file" ]; then
                # Replace occurrences in config.cfg
                # Replace "one-import-onnx" with "one-import-tflite"
                sed -i 's/one-import-onnx/one-import-tflite/g' "$config_file"
                
                # Get the prefix from the .tflite file
                for tflite_file in "$dir"*.tflite; do
                    prefix=$(basename "$tflite_file" .tflite)
                    break  # Take the first .tflite file's prefix and break
                done
                
                # Replace the prefix of .onnx in config.cfg
                sed -i "s|${prefix}.onnx|${prefix}.tflite|g" "$config_file"
                
                # Replace all occurrences of .onnx with .tflite
                sed -i 's/\.onnx/\.tflite/g' "$config_file"
                
                echo "Updated $config_file in $dir"
            else
                echo "No config.cfg found in $dir"
            fi
        else
            echo "No .tflite files found in $dir"
        fi
    fi
done

echo "Task completed."
