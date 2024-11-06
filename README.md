#!/bin/bash

# Create a folder named 'txt_files' in the current directory (if not already present)
mkdir -p ./txt_files

# Loop through all folders in the current directory
for dir in */; do
    # Check if it's a directory
    if [ -d "$dir" ]; then
        # Change to the directory
        cd "$dir" || continue

        # Find any .onnx or .tflite file
        model_file=$(find . -maxdepth 1 -type f \( -name "*.onnx" -o -name "*.tflite" \) | head -n 1)

        # If a model file exists
        if [ -n "$model_file" ]; then
            # Get the prefix of the model file (filename without extension and path)
            prefix="${model_file##*/}"
            prefix="${prefix%.*}"

            # Set the output .txt file name and path in the 'txt_files' folder
            txt_file="../txt_files/${prefix}.txt"

            # Create and write to the .txt file
            {
                echo "[network]"
                # Find and list any .tvn files in this directory
                for tvn_file in *.tvn; do
                    if [ -f "$tvn_file" ]; then
                        echo "$tvn_file"
                    fi
                done
                echo "[input]"
                echo "input.0.tv2b"
                echo "[golden]"
                echo "output.0.tv2b"
                echo "[output]"
                echo "output0.dat"
            } > "$txt_file"
        fi

        # Return to the parent directory
        cd ..
    fi
done

echo "Script completed."
