# Contributor's guide

`GES-echem-suite` is an open source project and, as such, all the contributions are well accepted. If you want to contribute to the library, a simple guide on how to interface with the GitHub repository is provided in what follows.

## General development process

* If you are a first-time contributor:

    * Go to [https://github.com/GES-compchem/GES-echem-suite](https://github.com/GES-compchem/GES-echem-suite) and click the “fork” button to create your own copy of the project repository.

    * Clone the project to your local computer using the command (where `<YOUR_USERNAME>` represent your personal GitHub username): 
        ```
        git clone https://github.com/<YOUR_USERNAME>/GES-echem-suite
        ```

    * Enter the reposiotry directory using the command:
        ```
        cd GES-echem-suite
        ```

    * Add the upstream repository using the command:
        ```
        git remote add upstream https://github.com/GES-compchem/GES-echem-suite.git
        ```

    * Now, when running the `git remote -v` command, the following reposiotries shold be visible:
        * `upstream`, which refers to the GES-compchem repository
        * `origin`, which refers to your personal fork

* Developing your contributions:
    
    * Pull the latest changes from upstream:
        ```
        git checkout main
        git pull upstream main
        ```

    * Create a branch for the feature you want to work on. Since the branch name will appear in the merge message, use a sensible name.:
        ```
        git checkout -b the_name_of_the_branch
        ```

    * Commit locally as you progress (`git add` and `git commit`) Use a properly formatted commit message. If possible, write tests that fail before your change and pass afterward, run all the tests locally (use `tox` to verify the compatibility with all the supported version of python). Be sure to document any changed behavior in docstrings, keeping to the [NumPy docstring standard](https://numpydoc.readthedocs.io/en/latest/format.html). Sphinx annotation are very welcome. If brand new functionality are added to the package make sure to update the documentation as well.

* To submit your contribution:

    * Push your changes back to your fork on GitHub:
        ```
        git push origin the_name_of_the_branch
        ```
    * Follow the GitHub authentication process.

    * Go to GitHub. The new branch will show up with a green Pull Request button. Make sure the title and message are clear, concise, and self- explanatory. Then click the button to submit it.

    * Ask for review from the development team.


## Codebase overview

The purpose of the library is to give to the user a simple, self-contained, interface to process experimental data derived from electrochemistry experiments. The library is divided into two modules:

* The `cellcycling` module: dedicated to the analysis of battery cycling experiments
* The `cyclicvoltammetry` module: dedicated to the analysis of cyclic-voltammetry experiments

#### Structure of the `cellcycling` module

The `cellcycling` module is divided into two submodules:

* The `cycles` module: defining the structure of the data associated to a cell-cycling experiment.
* The `read_input` module: dedicated to the parsing of experimental files given by the user.

The `cycles` module is design to mimic the structure of a cell-cycling experiment. The cell-cycling experiment is defined by a `CellCycling` class which internally carries a variable number of `Cycle` objects. Each `Cycle` object represents a complete charge/discharge battery cycle and, as such, it is composed by two `HalfCycles`, one for the charge and one for the discharge. A detailed description of all the attributes of each object can be found in the [API documentation](API-cellcycling-cycles).

The task of constructing a `CellCycling` experiment form experimental data files is given to the `read_input` module. The main object in the `read_input` module is represented by the `FileManager` class. The class accepts experimental data both in the form of files and bytestreams and store them in a `BytesIO` format. The `FileManager` class is capable of ordering cronologically subsequent experiments, associating charge and discharge half-cycles in a single charge/discharge cycle, fusing partial halfcycles eventually derived from interrupted experiment.A detailed description of all the attributes of the `FileManager` object together with a description of the associated legacy functions can be found in the [API documentation](API-cellcycling-read_input).

A schematic representation of the core components of the `cellcycling` module is provided in the following illustration:

```{image} ./images/developer_guide_scheme_cellcycling.png
:alt: developer_guide_scheme_cellcycling
:class: bg-primary mb-1
:width: 600px
:align: center
```

## Directory Structure

The directroy structure of the repository is the following:

```
.
├── bld.dat                 # File required by the conda package manager   
├── build.sh                # File required by the conda package manager   
├── docs                    # Folder containing the doumentation files
├── echemsuite              # Main folder containing the source code of the library
├── LICENSE                 # Licence file
├── meta.yaml               # Metadata required by the conda package manager
├── pyproject.toml          # Configuration file required by the pip package manager
├── README.md               # The repo README file
├── requirements.txt        # The list of requirements to use the package
├── setup.py                # The setup configuration file for the build tool
├── tests                   # The folder conatining all the test of the library
└── tox.ini                 # The tox configuration file to be used during automatic testing
```

The package folder is organized as follows:

```
.
├── cellcycling             # The folder of the cell-cycling module
│   ├── cycles.py           # The cycles submodule
│   ├── __init__.py
│   └── read_input.py       # The file manager module
├── cyclicvoltammetry       # The folder of the cyclic-voltammetry submodule
│   ├── cvanalysis.py       # The module containing the tools to analyze a cyclic voltammetry experiment
│   ├── __init__.py
│   └── read_input.py       # The module containing the parser for the experimental files
├── __init__.py
└── utils.py                # The script containing useful helper functions
```

The detailed information about the working of each module is provided in the [API documentation](API_Reference).