# book-notes


#!/bin/bash

# Loop through all directories in the current directory
for dir in */; do
    # Check if it's a directory and contains config.cfg
    if [ -d "$dir" ] && [ -f "$dir/config.cfg" ]; then
        echo "Processing $dir"
        
        # Find the first .onnx file in the directory (if any)
        onnx_file=$(find "$dir" -maxdepth 1 -name "*.onnx" | head -n 1)
        
        if [ -n "$onnx_file" ]; then
            # Get the base name of the .onnx file (without extension)
            onnx_base=$(basename "$onnx_file" .onnx)

            # Replace 'OCTnet_v1' with the .onnx base name in config.cfg
            sed -i "s/OCTnet_v1/$onnx_base/g" "$dir/config.cfg"
            echo "Replaced OCTnet_v1 with $onnx_base in $dir/config.cfg"
            
            # Check if there's a .tflite file in the folder
            tflite_file=$(find "$dir" -maxdepth 1 -name "*.tflite" | head -n 1)
            
            if [ -n "$tflite_file" ]; then
                # Replace '.onnx' with '.tflite' in config.cfg
                sed -i "s/\.onnx/\.tflite/g" "$dir/config.cfg"
                echo "Replaced .onnx with .tflite in $dir/config.cfg"
            fi
        else
            echo "No .onnx file found in $dir"
        fi
    else
        echo "No config.cfg found in $dir"
    fi
done

echo "Task completed."
