GSoC'25 Proposal - TARDIS RT Collaboration

<!-- <div style="display: flex"> -->
![gsoc banner](./GSoC-logo-horizontal.svg) 
<!-- </div> -->

# CARSUS DASHBOARD

## Project Details

- Organization : [TARDIS RT Collaboration](https://github.com/tardis-sn)
- Project Title : [CARSUS DASHBOARD](https://tardis-sn.github.io/summer_of_code/ideas/#carsus-dashboard)

- Mentor : [Josh Shields](https://github.com/jvshields)

- Difficulty : HARD

## Personal Information

Name : Karthik Rishinarada

Email : karthikrk11135@gmail.com

LinkedIn: [Karthik Rishinarada](https://www.linkedin.com/in/karthik-rishinarada-a61b39251/)

Github id : [karthik11135](https://github.com/karthik11135)

Brief Summary : I'm a senior-year college student from NIT Trichy, India. I interned at Capital One, where I worked with a Python team. Over the past few months, I have developed an interest in open source and started exploring open-source codebases. I enjoy listening to music, window shopping, and planning vacations.

Resume : [Link to my Resume](https://drive.google.com/file/d/1LTs82Yv-aLM0iVrQHyoPOCQsfxLe_wFC/view?usp=sharing)

## <u>About the Organisation</u>

TARDIS (Temperature And Radiative Diffusion In Supernovae) is an open-source radiative transfer code that is developed for simulating supernova spectra. TARDIS helps astrophysicsts understand how exploding stars appear to observers by creating synthetic spectra - essentially predicting what these star explosions would look like through a telescope. Rather than taking days or weeks to simulate a spectrum, TARDIS uses real approximations to generate while maintaining accuracy.

<u>Core Components of TARDIS</u>
| **Components** | **Description** |
|-------------------------------------|------------------------------------------------------------------|
| **Configuration & Input** | <ul><li>Handles model parameters, atomic data, and abundance configurations (YAML or HDF).</li></ul> |
| **Physical Model Setup** | <ul><li>Establishes the radial grid structure (ex: Radial 1D Geometry) and initial conditions.</li><li>Contains density profiles and abundance distributions across shells.</li></ul> |
| **Monte Carlo Radiation Transport** | <ul><li>Implements core Monte Carlo setup for packet propagation through shells.</li><li>Handles packet interactions including scattering, absorption, and re-emission.</li></ul> |
| **Plasma Physics** | <ul><li>Calculates ionization states and level populations.</li><li>Handles atomic transitions and electron density calculations.</li></ul> |
| **Opacity Calculation** | <ul><li>Computes line and continuum opacities for each shell.</li>
| **Monte Carlo Iteration Handler** | <ul><li>Controls the main iteration loop for temperature convergence. Once the convergence happens the data is used to update plasma properties</li></ul> |
| **Spectrum Synthesis** | <ul><li>Collects energy packets into observable spectra.</li>

<u> Flow Diagram </u>
![flow diagram of tardis](./tardisflow.png)

## About CARSUS

Carsus is a package developed by the TARDIS team to manage and analyse atomic datasets. Carsus collects data from various sources like CHIANTI, NIST etc. It converts this data for use in radiative transfer code like TARDIS. It contains readers/parsers for each data source which perform initial validation and parsing. Carsus also handles unit conversions and data transformations. It prepares data in specific formats (CollisionsPreparer, LevelLinesPreparer etc)

Carsus enables researchers to:

- Analyze atomic energy levels and atomic energy lines
- Investigate element composition in astrophysics
- Compare and validate data from different atomic databases
- Generate custom atomic datasets for specific research needs

<u>Flow Diagram</u>
![carsus flow chart](./carsusflow.png)

## <u>Project Summary</u>

The traditional way of getting insights from various atomic datasets is to run the scripts of carsus and look at the tables. Astrophysicsts and researchers spend time in creating their custom atomic files, comparing two atomic files etc. This manual process is time-consuming and can slow down research progress.

The Project - CARSUS Dashboard aims to streamline this workflow by providing a web-based application that facilitates numerous use cases. By enabling web access to different datasets, the project reduces the effort required to interact with Carsus, making data analysis faster. Web access to various datasets will lessen the amount of time researchers have to deal with carsus to get insights. Built using Flask and Jinja2, the application offers multiple API endpoints that allow users to request and visualize insights about atomic data in a nicely formatted way.

Some of the features of the application include:

1. Filtering data according to atomic numbers, ascending/descending order of wavelengths, line/level ids etc.
2. Supports viewing existing atomic datasets. 
3. Provides features to compare different atomic datasets, helping researchers validate data across sources.
4. System is designed to support both legacy atomic files and newer formats, ensuring long-term usability.
5. Includes thorough documentation and test coverage to maintain ease of use.

---

## First Objective Task

Task : Use Jinja2 to generate an HTML Report that investigates an atomic file. Display top 50 rows of levels and lines dataframes from the atomic file for Silicon

Link to the github repository : [Solution](https://github.com/karthik11135/carsus-dashboard)

<u>Explanation</u> : Custom Atomic file is created using NIST's atomic weights and ionization energies. GFALL Reader is used to get the data of lines and levels for the Silicon atom. TARDISAtomData class is used to create the atom data. This atom data is loaded on the first server load. When the user hits the endpoint (/get_data) the first 50 rows of both lines and levels data is outputted on the screen.

<u>Screenshots of the data</u>

<!-- ![lines screenshot](./linesfirstobj.png)
![levels screenshot](./levelsfirstobj.png) -->

| Lines               | Levels               |
| ------------------- | -------------------- |
| ![](./linesweb.png) | ![](./levelsweb.png) |

---

## My Approach towards the project

Tech Stack : Flask, Jinja2 and TailwindCSS for styling

On the server side, the default atom data is created using the default options. Various endpoints can be created to serve the atomic files data, compare different atomic datasets and also alter the default atomic dataset by sending specific options.

#### Backend Features

#### 1. Fetching data

Python API endpoints:
| Endpoint Name | HTTP Method | URL Pattern |
|------------------------------------|------------|---------------------------------------------------------------|
| Retrieve Lines Data | GET | `/get_lines_data?atomic_file_name=` |
| Retrieve Levels Data | GET | `/get_levels_data?atomic_file_name=` |
| Retrieve Collisions Data | GET | `/get_collisions_data?atomic_file_name=` |
| Retrieve Ionization Energies | GET | `/ionization_energies?atomic_file_name=&atomic_number=&ion_number=&min_energy=` |
| Retrieve Macro Atom Data | GET  | `/macro_atom?atomic_file_name=&atomic_number=&ion_number=&min_transition_probability=` |
| Retrieve Collisions Data | GET  | `/collisions_data?atomic_file_name=&atomic_number=&ion_charge=&min_energy=&max_energy=` |
| Retrieve Cross Sections Data | GET | `/cross_sections?atomic_file_name=&atomic_number=&ion_charge=&min_energy=` |

GET: Retrieves data when a valid atomic_file_name is provided.

#### 2. Comparing Atomic Files

Python API endpoints:
| Endpoint Name | HTTP Method | URL Pattern | Body |
|------------------------------------|------------|---------------------|---------------|
| Fetch key differences | GET | `/key_diff?atomic_file1=&atomic_file2=&diff_in=linesorlevels` | `NA` |
| Fetch ion differences | POST | `/ion_diff??atomic_file1=&atomic_file2=` | `{key_name: string, ion: string or tuple, rtol: int, simplify_output=bool, return_summary=bool, style=bool, style_axis=int}` |
| Fetch the plot for ion differences | POST | `/plot_ion_diff?atomic_file_name1=&atomic_file_name2=` | `{key_name: string, ion: string or tuple, column: string}` |

#### Frontend Features

- Users can browse specific parts of the atomic datasets.

- Filtering options for atomic numbers, wavelengths, and metadata.

- Comparison tools to analyze differences between datasets.

---

### Milestones

| Sno | Milestone                                  | Focus Area                                                                                | Deliverables                                                                                                                                                                                  |
| --- | ------------------------------------------ | ----------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 1   | Backend Setup                              | Implementing basic API endpoints for fetching atomic data and writing relevant test cases | <ul><li>Working Flask app with GET endpoints (/get_lines_data, /get_collisions_data, /get_levels_data)</li><li>A simple frontend page using Jinja2 templates</li></ul>                        |
| 2   | Data Fetching and setting up POST requests | Frontend features incorporated       | <ul><li>Fully functional data-fetching API </li><li>Frontend with forms and data filtering</li><li>Initial test suite for API endpoints</li></ul> |
| 3   | Comparing Atomic Datasets                  | Comparison APIs setup, visualization                                                      | <ul><li>Working comparison APIs with visualization</li><li>Frontend for comparing datasets</li><li>Tests for endpoints</li></ul>                                                              |
| 4   | Documentation and Testing                  | Docs, tests, final deployment                                                             | <ul><li>Documentation of entire code workflow </li><li>Full test coverage</li><li>Deployable project</li></ul>                                                                                |

---

### Why I've chosen TARDIS?

Over the past few months I wanted to explore open-source contributions. So I started looking for python repositories on GitHub. That’s when I came across TARDIS.

I've gone through the entire codebase and gained an understanding of its main components. While going through the code, I encountered numerous scientific terms like Monte Carlo Iteration, Plasma State, JBlues etc. I really enjoyed the process of googling these terms and understanding them. The documentation was written in a very clear way. The clarity of explanations in the documentation made it easier to understand the code. These are the two primary things that made me stick to contributing to TARDIS.

---

### Why am I the best fit?

My first internship offer was from a fintech company — Capital One, where I worked as a Software Developer Intern for two months. During this time, I improved my Python programming skills, which is the primary language used in TARDIS. I improved the runtime of our codebase by 40%. After my internship, I discovered TARDIS and began exploring its code by studying the documentation and working through the quickstart tutorials. I've gone threw few issues in the codebase and tried to solve a few of them.

Main Tardis repo:

- [add test to line info](https://github.com/tardis-sn/tardis/pull/2947)
- [from Simulation test for RPacket Plot](https://github.com/tardis-sn/tardis/pull/2945)
- [TODO: Tests of Composition](https://github.com/tardis-sn/tardis/pull/2944)
- [TODO: Rename tau to tau_event](https://github.com/tardis-sn/tardis/pull/2931)
- [Tests for config validator](https://github.com/tardis-sn/tardis/pull/2926)
- [refactor: Remove ConfigWriterMixin class](https://github.com/tardis-sn/tardis/pull/2926)

Regression Data repo:

- [Regression data for Composition](https://github.com/tardis-sn/tardis-regression-data/pull/44)

Carsus repo:

- [fixes NISTWeightsComp initialization error](https://github.com/tardis-sn/carsus/pull/436)

Apart from this, I actively explore new technologies and work on personal projects to enhance my skills. I believe that I am a perfect fit for this project because I've a good grip on the codebase and I understand what happens under the hood. I also believe that I can hit the ground running from day 0.

---

### Conclusion

In conclusion, my strong knowledge in Python, experience with open-source contributions, and familiarity with the TARDIS codebase make me a great fit for this project. I'm motivated to develop the CARSUS Dashboard, ensuring it becomes a valuable tool for astrophysicists. While I have exams from May 3rd to May 9th, I will be entirely free afterward and fully committed to executing the project. I look forward to contribute meaningfully to the TARDIS RT Collaboration.
