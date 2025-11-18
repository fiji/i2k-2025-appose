---
theme: default
background: https://images.unsplash.com/photo-1451187580459-43490279c0fa?q=80&w=2072
title: Appose Workshop
info: |
  ## Appose: Interprocess Cooperation with Shared Memory
  I2K 2025 Workshop

  Teaching participants how to use Appose with Fiji for Python integration
class: text-center
drawings:
  persist: false
transition: slide-left
mdc: true
duration: 120min
---

# Appose Workshop

Interprocess Cooperation with Shared Memory

<div class="pt-12">
  <span @click="$slidev.nav.next" class="px-2 py-1 rounded cursor-pointer" hover="bg-white bg-opacity-10">
    Press Space to begin <carbon:arrow-right class="inline"/>
  </span>
</div>

---
layout: default
---

# What is Appose?

**"Interprocess cooperation with shared memory"**

<v-clicks>

- 🔄 **Interprocess** - Multiple processes connected via communication protocol
- 🤝 **Cooperation** - Build environments, start services, run tasks
- 💾 **Shared Memory** - Cross-platform, cross-language memory buffers

</v-clicks>

---
layout: two-cols
---

# Interprocess

Multiple **processes** (programs) connected via communication protocol

<v-clicks>

**Examples:**
- Fiji (Java) → Python program
- napari (Python) → Java program
- Java ↔ Java
- Python ↔ Python
- Python ↔ R

</v-clicks>

::right::

<v-clicks>

### Pros ✅
- Called program doesn't need to "know about" caller
- Clean separation of concerns
- Use best tool for each job

### Cons ❌
- Each process has own memory space
- No direct memory sharing (traditionally)

</v-clicks>

---
layout: default
---

# Cooperation

How Appose facilitates interprocess cooperation:

<v-clicks depth="2">

1. **Build Environment**
   - Using pixi, uv, or micromamba
   - No pre-installation needed
   - Everything downloaded on demand

2. **Start Service**
   - Worker process running in that environment
   - Stays alive for multiple tasks

3. **Run Tasks**
   - Feed inputs, receive outputs
   - Via JSON serialization
   - Through the service/worker

</v-clicks>

---
layout: default
---

# Shared Memory

Cross-platform, cross-language support for named shared memory buffers

<v-clicks>

**Python Implementation:**
```python
from multiprocessing.shared_memory import SharedMemory
```
Extension of Python's built-in shared memory

**Java Implementation:**
```java
import org.apposed.appose.SharedMemory;
```
New class modeled closely after Python's implementation

**Result:** Efficient transfer of large data (images!) without serialization overhead

</v-clicks>

---
layout: center
class: text-center
---

# Live Demos

Real-world Appose integrations in action

---
layout: two-cols
---

# Demo 1: TrackMate

Deep learning spot detectors

- v9-appose branch
- Python-based detection in Java application
- Real-time particle tracking

::right::

# Demo 2: SAMJ

One-click live segmentation

- Segment Anything Model integration
- Python AI model called from Java
- Interactive segmentation UI

---
layout: default
---

# Demo 3: Mastodon

Large-scale tracking capabilities

Advanced tracking algorithms powered by Appose

_(Demo with Stefan's assistance)_

---
layout: center
class: text-center
---

# Hands-On Workshop

**Build your own Appose-powered tool in Fiji**

<div class="pt-4">
Goal: Integrate UNSEG with Fiji via Appose
</div>

<div class="pt-8 text-sm opacity-75">
⏱️ ~90 minutes
</div>

---
layout: default
---

# Workshop Overview

**What we'll build:**

Fiji plugin (Groovy script) that calls UNSEG for nucleus/cell segmentation

**UNSEG:** Unsupervised segmentation of cells and their nuclei in tissue
https://github.com/uttamLab/UNSEG

<v-clicks>

**What you'll learn:**
- Setting up Python environments with pixi
- Adapting Python code for Appose
- Writing Fiji scripts that call Python
- Debugging cross-language integration

</v-clicks>

---
layout: default
---

# Prerequisites

**Required:**
- Fiji installed

**Recommended:**
- bash terminal
- git
- pixi
- claude (or your favorite AI assistant)

**Can follow along without coding - just watch!**

---
layout: default
---

# Step 0: Create Workspace

Create a dedicated folder for this project

```bash
mkdir ~/Desktop/unseg-fiji
cd ~/Desktop/unseg-fiji
```

<v-click>

**💡 Tip:** Keep each workshop project in its own folder to avoid confusion

</v-click>

<v-click>

**🎯 Checkpoint:** You should now be in an empty `unseg-fiji` directory

</v-click>

---
layout: default
---

# Step 1: Clone UNSEG

Get the UNSEG repository from GitHub

```bash
git clone https://github.com/uttamLab/UNSEG
cd UNSEG
```

<v-click>

**What's in here?**
- `unseg.py` - The segmentation algorithm
- `run_unseg.ipynb` - Jupyter notebook showing how to use it
- `requirements.txt` - Python dependencies
- `image.zip` - Sample image data

</v-click>

---
layout: default
---

# Step 2: Create Test Script

Extract code from the Jupyter notebook into a Python script

**Manual approach:**
1. Go to https://github.com/uttamLab/UNSEG in browser
2. Click into `run_unseg.ipynb`
3. Copy code blocks into text editor
4. Save as `run.py`

<v-click>

**Or use your AI assistant!** 🤖

</v-click>

---
layout: default
---

# Step 3: Set Up Pixi Environment

Install pixi if needed: https://pixi.sh/latest/installation/

<v-click>

Initialize pixi project and import dependencies:

```bash
pixi init
pixi import requirements.txt --format pypi-txt --environment default
```

</v-click>

<v-click>

**This creates `pixi.toml`** - the environment specification

</v-click>

---
layout: default
---

# Step 3: Configure Dependencies

Edit `pixi.toml` to optimize package sources

<v-click>

**Move packages from `pypi-dependencies` to `dependencies`**
- Prefer conda-forge when possible (faster, more reliable)
- Keep PyPI-only packages in `pypi-dependencies`

</v-click>

<v-click>

**Common issues to fix:**
1. `opencv-python` fails → move back to `pypi-dependencies`
2. Missing images → run `unzip image.zip`
3. Qt errors → change `opencv-python` to `opencv-python-headless`

</v-click>

<v-click>

**Test it:**
```bash
pixi run python run.py
```

</v-click>

---
layout: default
---

# Step 4: Adapt unseg.py for Appose

Make the library "listenable" by adding a callback mechanism

<v-click>

Add at the top of `unseg.py`:

```python
report = print

def listen(callback):
    global report
    report = callback
```

</v-click>

<v-click>

Then replace all `print(` with `report(`

**Why?** Allows calling code to capture progress updates via callback

</v-click>

---
layout: default
---

# Step 4: Add Running Code

Paste the contents of `run.py` at the bottom of `unseg.py`

**Now we have a single file that:**
- Defines the library functions
- Includes example usage code
- Can be called as a script

<v-click>

**💡 This makes it easy to test the code standalone before integrating with Appose**

</v-click>

---
layout: default
---

# Step 5: Add Appose Infrastructure

Add Appose task handling at the bottom of `unseg.py`:

```python
if 'task' not in globals():
    import appose.python_worker
    task = appose.python_worker.Task()

listen(task.update)
```

<v-click>

**What this does:**
- Checks if running in Appose context (task exists) or standalone
- Creates task object when running via Appose
- Connects our `listen` callback to Appose's `update` method

</v-click>

---
layout: default
---

# Step 6: Integrate with Appose

Modify `unseg.py` to work in "Appose mode"

<v-clicks depth="2">

**Changes needed:**

1. **Get input from task instead of file:**
   ```python
   if 'task' in globals():
       image = task.inputs['image']
   else:
       image = read_from_file()
   ```

2. **Comment out plot calls** (or make conditional)

3. **Set outputs:**
   ```python
   if 'task' in globals():
       task.outputs['mask_nuclei'] = mask_nuclei
       task.outputs['mask_cells'] = mask_cells
   ```

</v-clicks>

---
layout: default
---

# Step 7: Write the Fiji Plugin

Now the exciting part - call Python from Java! 🎉

Create a new Groovy script in Fiji's Script Editor

**Why Groovy?**
- Full Java compatibility
- Works seamlessly with Fiji
- Simpler syntax than pure Java

---
layout: default
---

# Step 7: Import and Annotate

```groovy
import org.apposed.appose.Appose

#@ Img image
#@output Img nuclei
#@output Img cells
```

<v-click>

**What this does:**
- Imports Appose library
- Uses SciJava script parameters for input/output
- `#@` annotations create UI automatically

</v-click>

---
layout: default
---

# Step 7: Build Environment

Embed the pixi.toml configuration:

```groovy
println("== BUILDING ENVIRONMENT ==")
pixiToml = """
[project]
name = "unseg"
version = "0.1.0"
channels = ["conda-forge"]
platforms = ["linux-64", "osx-arm64", "win-64"]

[dependencies]
# ... paste your pixi.toml dependencies here ...

[pypi-dependencies]
opencv-python-headless = "*"
"""

env = Appose.pixi().content(pixiToml).logDebug().build()
println("Environment build complete!")
```

---
layout: default
---

# Step 7: Read Python Script

Load your adapted `unseg.py`:

```groovy
// Read the Python script
unsegPath = "/full/path/to/UNSEG/unseg.py"
unsegScript = new File(unsegPath).text
```

<v-click>

**💡 Later:** You could embed the Python code directly in the Groovy script, or package it as a resource

</v-click>

---
layout: default
---

# Step 7: Execute Task

Run the Python code via Appose:

```groovy {all|2|3-4|5|6-8|10}
println("== STARTING PYTHON SERVICE ==")
try (python = env.python()) {
    inputs = ["image": image]
    task = python.task(unsegScript, inputs)
        .listen(println)
        .waitFor()

    println("TASK FINISHED: ${task.status}")
    nuclei = task.outputs["mask_nuclei"]
    cells = task.outputs["mask_cells"]
}
finally {
    println("== SHUTTING DOWN ==")
}
```

---
layout: center
class: text-center
---

# Let's Code! 👨‍💻👩‍💻

**Follow along or just watch**

We'll go through steps 0-7 together

<div class="pt-8 text-sm opacity-75">
Stuck? Clone reference repo:
<div class="pt-2 font-mono">
git clone https://github.com/ctrueden/unseg-fiji
</div>
Use tags to jump to any step:
<div class="pt-2 font-mono">
git checkout step-3
</div>
</div>

---
layout: default
---

# Troubleshooting Tips

**Common issues:**

<v-clicks>

- **Environment build fails** → Check pixi.toml syntax, verify channels
- **Python script errors** → Test standalone first with `pixi run python unseg.py`
- **Input/output types** → Verify Appose can serialize your data types
- **Memory issues** → Use shared memory for large images
- **Service won't start** → Check pixi installation, try `logDebug()` mode

</v-clicks>

<v-click>

**Debug strategy:**
1. Test Python code standalone
2. Test environment with simple script
3. Add Appose integration incrementally

</v-click>

---
layout: default
---

# Next Steps

**After this workshop:**

<v-clicks>

- Explore Appose examples: https://github.com/apposed/appose
- Try other Python libraries in Fiji
- Integrate Appose into your own projects
- Join the discussion: https://forum.image.sc (tag: appose)
- Contribute back! PRs welcome

</v-clicks>

<v-click>

**Ideas for experimentation:**
- Different deep learning models
- R-based statistical analysis
- Custom Python algorithms
- Bidirectional workflows (Python calling Java)

</v-click>

---
layout: center
class: text-center
---

# Questions?

Thank you for participating! 🙏

<div class="pt-8">
<div>Appose: https://github.com/apposed/appose</div>
<div>UNSEG Reference: https://github.com/ctrueden/unseg-fiji</div>
<div>Forum: https://forum.image.sc</div>
</div>

<div class="pt-12 text-sm opacity-75">
Slides available at: [add your URL here]
</div>
