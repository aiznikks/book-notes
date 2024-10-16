#!/bin/bash

# Loop through all subdirectories
for dir in */; do
  # Check if the directory exists
  if [ -d "$dir" ]; then
    echo "Entering directory: $dir"

    # Check if config.cfg exists in the subdirectory
    if [ -f "$dir/config.cfg" ]; then
      echo "Found config.cfg in $dir, running onecc profile -C config.cfg"

      # Save the log file in the respective subdirectory
      log_file="${dir%/}/onecc_profile.log"
      
      # Run the onecc command and redirect both stdout and stderr to the log file
      if (cd "$dir" && onecc profile -C config.cfg > "$log_file" 2>&1); then
        echo "Successfully ran onecc in $dir, log saved to $log_file"
      else
        echo "Error running onecc in $dir, check $log_file for details"
      fi
    else
      echo "config.cfg not found in $dir, skipping..."
    fi
  else
    echo "$dir is not a directory, skipping..."
  fi
done
