# Timeline Summary of the Rust for CPython Project

The Rust for CPython project ramped up its coordination with the Rust team and focused heavily on foundational technical and community-building efforts from late 2025.

### April 2026: Project Finalizing for Public Presentation

* **Apr 20, 2026:** Held the practice session for the [PyCon US](https://us.pycon.org/2026/) talk.  
* **Apr 13, 2026:** Focused on deciding on a module to port.  
* **Apr 6, 2026:** Discussed progress on the Windows build and the selection of a module to include in Python 3.16.

### Early 2026: Technical Development and Coordination

* **Mar 16, 2026:** Updates included working on a Windows build system patch and preparing a sanitizers PR. There were plans to set up the public Discord and begin practicing the [PyCon US](https://us.pycon.org/2026/) talk.  
* **Mar 5, 2026:** Discussed the "Drop with context" feature for improved deallocation ergonomics and the incompatibility issues between LLVM/Rust and GCC/Clang sanitizers, emphasizing that stabilizing the sanitizer ABI is critical.  
* **Mar 2, 2026:** Reported a build system milestone: WASI, Android, macOS, and Linux crossbuilds were now working with a clean symbol check. Scheduled a meeting with the Steering Council for April 2, 2026\.  
* **Feb 26, 2026:** Coordinated a two-person (Rust \+ CPython) talk for [RustConf](https://rustconf.com/), and continued exploring how to link Rust libraries (rlibs) externally using a C linker.  
* **Feb 16, 2026:** Agreed to post a status update to the community and open up the project's Discord server.  
* **Feb 5, 2026:** Held a pivotal discussion with a member from the Rust for Linux (R4L) project, who provided advice on securing community buy-in (starting small, focusing on quality) and managing governance by securing flexibility from the Steering Council.  
* **Jan 29, 2026:** Discussed strategies for introducing C programmers to Rust and continued technical discussion on adding split linker arguments (e.g., rustc-link-lib-bins, rustc-link-lib-tests) to Cargo for conditional static/dynamic linking.  
* **Jan 22, 2026:** Addressed the technical challenge of safely modeling Python objects in Rust due to reference counting tied to the Python interpreter state, which leads to soundness edge cases when a thread detaches. The team decided to use the [PyO3](https://pyo3.rs/) Bound\<'py, T\> API and explore collaboration on future Rust features like "async drop" and linear typing.  
* **Jan 8, 2026:** Focused on build system issues related to static linking and the undesirability of static libraries exporting Rust std symbols. Identified the CPython-Rust cyclical bootstrap dependency as a critical issue for distribution maintainers, proposing to document the minimal steps for Rust's bootstrap process to allow for non-Python alternatives.

### Late 2025: Initial Planning and Goals

* **Dec 18, 2025:** Discussed a workaround for linking static Rust libraries into shared objects and addressed long-term goals for stabilizing the Rust ABI and managing the MSRV policy to align with Python's LTS (Long-Term Support) policy.  
* **Dec 15, 2025:** Submitted the [PyCon US](https://us.pycon.org/2026/) talk proposal and established a potential timeline targeting a **Python 3.15b1 hard deadline of May 6, 2026**. The timeline planned to implement the safe abstraction API and start the PEP in January, implement \`bzip2\`/\`base64\` and propose the PEP in late February/early March, and request a decision from the Steering Council in April.  
* **Dec 11, 2025:** Raised concerns about Python's support for non-Tier 1/2 platforms and Rust's toolchain, requiring reliance on alternative backends like gccrs. Action items were set to list unsupported platforms and explore an official Python Tier 4 for community-supported platforms.  
* **Dec 8, 2025:** Held the first coordination meeting with the Rust team, discussing concerns like platform support differences (LLVM vs. GCC), unstable features (MSRV, Stable Rust ABI), and build system challenges (Cargo vendor, Python C API bindings).  
* **Dec 1, 2025:** Identified initial modules for Rust implementation: **Base64**, **Libbzip2-rs**, and the **Memoryview object**. Began discussions on defining safe C API abstractions and coordinating with the Rust team on unstable features and an MSRV.  
* **Nov 24, 2025:** Defined the goals for the PEP to highlight memory safety, fearless refactoring, and ergonomics. Decided to create a GitHub organization and plan the implementation by iterating on a safe C API abstraction.

