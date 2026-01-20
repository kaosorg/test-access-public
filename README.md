TODO: write readme for repo

## Recipe For A Release Package
1. Ensure release repo is pointing to updated develop branch
   1. `git pull origin develop`
2. Ensure submodules point to their latest develop branch
   1. `git submodule foreach 'git checkout develop'`
   2. `git submodule foreach 'git pull origin develop'`
3. Run `copy_submodule_files.sh` to generate a `src` folder in root
4. Open `src` and sort define files into correct layers
   - `task_managers` folder: 
      - `define_memory_sections.h`  
      - `define_schedule_entry.h`
   - `tasks` folder:
      - `define_error_callback.h`
      - `define_error_criticality.h`
      - `define_error_ids.h` 
      - `define_error_inject_reset.h` 
      - `define_tasks_config.h` 
   - `middleware` folder:
      - `define_error_flags.h`
5. Find all `#ifdef TEST` instances and delete them
   - **Note:** Make sure to also remove the #elif, #else and #endif where it applies
6. Update versioning in doxygen file headers in `src` folder
7. Run `gen_image_dox.sh` to ensure any new images are used.
8. Open safety_framework.X in MPLAB X and make sure that it builds on Os with XC8 2.49
   - Make sure the program works as expected.
      - Resets twice and then runs indefinitely in debug mode
9.   zip the MPLABX project
10. Add the project to the safety_framework forlder
11. Move the `src` folder into the safety_framework folder
12. Generate Doxygen documentation
   1. Open Doxywizard
   2. File -> Open -> Doxyfile (in release-package repo)
   3. Open `safety_framework/documentation.html` and check that the HTML documentation works as expected
13. Zip the safety_framework folder and ship it


## Scripts

### gen_image_dox.sh
Looks for images in the `safety_framework/docs/uml_diagrams` folder and, using the subfolders to decide which doxygen group it will be added to, and creates .dox files for each layer in the `doxygen_images` folder. It is assumed that `uml_diagrams` subfolders are named after a doxygen `@defgroup`, otherwise the images will not show up as expected in generated documentation. 

**Note:** This script will overwrite the following files:
  - images_architecture.dox
  - images_task_manager_layer.dox
  - images_task_layer.dox
  - images_middleware.dox
  - images_driver.dox

### copy_submodule_files.sh
TODO: Write my description