#!/bin/bash

# Loop through all subdirectories
for dir in */; do
  # Check if the directory exists
  if [ -d "$dir" ]; then
    echo "Entering directory: $dir"

    # Check if config.cfg exists in the subdirectory
    if [ -f "$dir/config.cfg" ]; then
      echo "Found config.cfg in $dir, running onecc -C config.cfg"
      
      # Attempt to run the command and capture any errors
      if (cd "$dir" && onecc -C config.cfg); then
        echo "Successfully ran onecc in $dir"
      else
        echo "Error running onecc in $dir"
      fi
    else
      echo "config.cfg not found in $dir, skipping..."
    fi
  else
    echo "$dir is not a directory, skipping..."
  fi
done
