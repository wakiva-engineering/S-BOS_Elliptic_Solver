```
          _____                            _____                   _______                   _____          
         /\    \                          /\    \                 /::\    \                 /\    \         
        /::\    \                        /::\    \               /::::\    \               /::\    \        
       /::::\    \                      /::::\    \             /::::::\    \             /::::\    \       
      /::::::\    \                    /::::::\    \           /::::::::\    \           /::::::\    \      
     /:::/\:::\    \                  /:::/\:::\    \         /:::/~~\:::\    \         /:::/\:::\    \     
    /:::/__\:::\    \                /:::/__\:::\    \       /:::/    \:::\    \       /:::/__\:::\    \    
    \:::\   \:::\    \              /::::\   \:::\    \     /:::/    / \:::\    \      \:::\   \:::\    \   
  ___\:::\   \:::\    \            /::::::\   \:::\    \   /:::/____/   \:::\____\   ___\:::\   \:::\    \  
 /\   \:::\   \:::\    \          /:::/\:::\   \:::\ ___\ |:::|    |     |:::|    | /\   \:::\   \:::\    \ 
/::\   \:::\   \:::\____\~~~~~~~~/:::/__\:::\   \:::|    ||:::|____|     |:::|    |/::\   \:::\   \:::\____\
\:::\   \:::\   \::/    /::::::::\:::\   \:::\  /:::|____| \:::\    \   /:::/    / \:::\   \:::\   \::/    /
 \:::\   \:::\   \/____/~~~~~~~~~~\:::\   \:::\/:::/    /   \:::\    \ /:::/    /   \:::\   \:::\   \/____/ 
  \:::\   \:::\    \               \:::\   \::::::/    /     \:::\    /:::/    /     \:::\   \:::\    \     
   \:::\   \:::\____\               \:::\   \::::/    /       \:::\__/:::/    /       \:::\   \:::\____\    
    \:::\  /:::/    /                \:::\  /:::/    /         \::::::::/    /         \:::\  /:::/    /    
     \:::\/:::/    /                  \:::\/:::/    /           \::::::/    /           \:::\/:::/    /     
      \::::::/    /                    \::::::/    /             \::::/    /             \::::::/    /      
       \::::/    /                      \::::/    /               \::/____/               \::::/    /       
        \::/    /                        \::/____/                 ~~                      \::/    /        
         \/____/                          ~~                                                \/____/         
```
<!-- Modified from art generated via patorjk.com/software/taag/ -->

# Elliptic Grid Smoothing
The code provided in this package is an Elliptic Grid Smoothing algorithm designed to smooth 2D CFD meshes by solving the 2D elliptic equations. A 2D NACA 2412 airfoil has been included for testing. The user can also read in their own 2D structured mesh in the Plot3D (.xyz) file format. The smoothed mesh will then be exported in Plot3D format with the extension "_smoothed" after the geometry name and before the file extension.

## Package Contents
- elliptic_solver.exe: Binary executable running the elliptic smoothing algorithm compiled for Windows 11
- elliptic_smoothing.in: Example user input file
- NACA_2412.xyz: 2D Plot3D NACA 2412 airfoil used as an example case to test
- LICENSE: Text based license file containing the user agreement

## Capabilities
Below are the capabilities of the code.

### Solver Types
- 2D Laplace Equations
- 2D Poisson Equations
  - Thomas-Middlecoff Source Terms: Attempt to maintain initial mesh nodal distribution
	- Steger-Sorenson Source Terms: Attempt to force boundary orthogonality while maintaining initial mesh boundary spacing
	- Hybrid Source Terms: Merges the Thomas-Middlecoff and Steger-Sorenson source term calculations

### Geometry Capabilities
- Plot3D (.xyz) Mesh: Can read any 2D mesh written in the standard Plot3D binary format

### Export Format
- Standard 2D Plot3D binary format

### Steger-Sorenson Distance Calculations
The Steger-Sorenson attempts to preserve the distance between the boundary nodes and the neighboring internal nodes of the original mesh. The code has two options to compute the distance. The first option is to use the exact distance between the boundary node and its neighboring internal node. The second approach is to project the distance between the boundary node and its neighboring node on to the unit normal vector located at the boundary node. The projection approach is computationally more expensive but yields better results for highly skewed meshes.

## User Input File
The user input file is a text-based file that must be labeled "elliptic_smoothing.in". Below is the expected format for the user input file. The content below shows the expected parameter name followed by the data type in prentices and then a colon. After the colon is a brief description of the parameter. The data type should be removed from the active user input file, and the variable descriptions should be replaced by the desired value of the variable.

### User Input File Parameter Definitions
// GEOMETRY PARAMETERS
geometry_name (string): The name of the geometry. This will be the expected input name of your custom geometry without the file extension. This will also be the name of the output geometry with the "_smoothed" extension written after the name.
I_boundary_type (integer): Set this to 0 if the boundaries in the I-direction are fixed. Set this to 1 if the boundaries in the I-direction are periodic. (user custom mesh only)
J_boundary_type (integer): Set this to 0 if the boundaries in the J-direction are fixed. Set this to 1 if the boundaries in the J-direction are periodic. (user custom mesh only)

// ELLIPTIC SOLVER PARAMETERS
source_term_generation (integer): Set to 0 for the Thomas-Middlecoff source terms. Set to 1 for the Steger-Sorenson source terms. Set to 2 for the Hybrid source terms. Set to -1 to solve the Laplace equation.
max_smoothing_iterations (integer): Max number of iterations to run the smoothing algorithm on the meth.
max_solver_iterations (integer): Max number of iterations to run the system of equations solver on the elliptic equations.
convergence_tolerance (decimal): The solver residual convergence tolerance
iterations_with_reduced_stencil (integer): Number of iterations using a reduced 5-point stencil instead of a 7-point stencil to help with the solver convergence.
solver_relaxation (decimal): Solver relaxation factor for updating the mesh coordinates between smoothing iterations.
ilut_p_largest_terms (integer): The number of largest terms to accept for the ILUT preconditioner matrix.
ilut_threshold (decimal): The solver threshold for the ILUT preconditioner matrix.

// STEGER-SORENSON PARAMETERS
iterations_ss_turned_off (integer): Number of iterations to turn off the Steger-Sorenson source terms.
iterations_ss_reduced_influence (integer): Number of iterations to reduce the influence of the Steger-Sorenson source terms.
use_projectiong_for_spacing (True/False): If True, uses the projected distance. If False, uses the distance between the boundary node and its neighboring interior node. See **Steger-Sorenson Distance Calculations** for additional details.
interpolation_damping_coefficient (decimal): Damping coefficient used for the interpolation of the source terms from the bounbdary to the internal nodes.
sharp_angle_threshold (decimal): The threshold in degrees to determine if the angle associated with a boundary node is considered sharp.

### Example User Input File
#### NACA 2412 Airfoil Example
// GEOMETRY PARAMETERS  
geometry_name: NACA_2412  
I_boundary_type: 0  
J_boundary_type: 1  
  
// ELLIPTIC SOLVER PARAMETERS  
source_term_generation: 2  
max_smoothing_iterations: 1000  
max_solver_iterations: 1000  
convergence_tolerance: 1e-8  
iterations_with_reduced_stencil: 0  
solver_relaxation: 0.7  
ilut_p_largest_terms: 5  
ilut_threshold: 1e-3  
  
// STEGER-SORENSON PARAMETERS  
iterations_ss_turned_off: 0  
iterations_ss_reduced_influence: 0  
use_projectiong_for_spacing: True  
interpolation_damping_coefficient: 0.7  
sharp_angle_threshold: 90  

#### Unit Triangle Example
// GEOMETRY PARAMETERS  
geometry_name: unit_triangle  
I_boundary_type: 0  
J_boundary_type: 1  
  
// ELLIPTIC SOLVER PARAMETERS  
source_term_generation: 2  
max_smoothing_iterations: 1000  
max_solver_iterations: 1000  
convergence_tolerance: 1e-8  
iterations_with_reduced_stencil: 0  
solver_relaxation: 0.7  
ilut_p_largest_terms: 5  
ilut_threshold: 1e-3  
  
// STEGER-SORENSON PARAMETERS  
iterations_ss_turned_off: 10  
iterations_ss_reduced_influence: 0  
use_projectiong_for_spacing: True  
interpolation_damping_coefficient: 0.7  
sharp_angle_threshold: 90
