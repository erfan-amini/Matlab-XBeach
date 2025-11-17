# XBeach Wave Decay Analysis and Solutions

## Summary of Issues

Your XBeach non-hydrostatic model is experiencing excessive wave decay due to a combination of **physical and numerical diffusion mechanisms**. The primary issue has been identified as **excessively high bed friction coefficients**.

## Root Causes Identified

### 1. CRITICAL: Bed Friction Too High (PRIMARY CAUSE)

**Current values in `bedfricfile.txt`:**
- Offshore section: `cf = 0.021`
- Onshore section: `cf = 0.01`

**Expected values for fine sand (D50 = 0.128 mm) in laboratory flumes:**
- Typical range: `cf = 0.002 - 0.005`
- Smooth flume bed: `cf = 0.001 - 0.003`

**Your current values are 5-10x too high!**

This excessive bed friction is causing massive wave energy dissipation, which explains:
- Progressive wave height decay at successive gauges
- Near-zero wave heights at WG4
- Inability to match measured water levels

### 2. Excessive Horizontal Viscosity

**Current:** `nuh = 0.01`
**Recommended:** `nuh = 0.001` (or even lower)

For short-period waves (T = 0.5-0.9s), high horizontal viscosity acts as a damping mechanism over long propagation distances. This compounds the bed friction problem.

### 3. Disabled Subgrid Turbulence Model

**Current:** `smag = 0`
**Recommended:** `smag = 0.1`

Completely disabling the Smagorinsky model removes scale-dependent viscosity, forcing reliance on constant `nuh` parameter which is less appropriate for variable-length waves.

### 4. Low CFL Number

**Current:** `CFL = 0.3`
**Recommended:** `CFL = 0.5` (test for stability)

While lowering CFL improved regularity, it also increases numerical diffusion by requiring more computational steps. There's a balance to strike.

### 5. Breaking Parameter

**Current:** Not explicitly set (defaults to ~0.6)
**Recommended:** `maxbrsteep = 0.8`

Your waves appear to break/decay prematurely. Allowing steeper waves before breaking may help.

## Solutions

### Priority 1: Fix Bed Friction (IMPLEMENT THIS FIRST)

Replace `bedfricfile.txt` with `bedfricfile_corrected.txt`:

```bash
cp bedfricfile_corrected.txt bedfricfile.txt
```

Or update your params file to use the corrected file:
```
bedfricfile = bedfricfile_corrected.txt
```

**New values:**
- Offshore: `cf = 0.003` (7x reduction)
- Onshore: `cf = 0.002` (5x reduction)

**Expected impact:** This alone should **dramatically reduce wave decay** and is likely to solve most of your problem.

### Priority 2: Reduce Horizontal Viscosity

In your params file, change:
```
nuh = 0.001
```

**Testing sequence:**
1. First test with `nuh = 0.001`
2. If waves are still decaying too much, try `nuh = 0.0005`
3. You can even try `nuh = 0.0` if numerical stability permits

### Priority 3: Enable Smagorinsky Model

Add to params file:
```
smag = 0.1
```

This provides scale-dependent turbulence modeling rather than uniform diffusion.

### Priority 4: Increase CFL (Carefully)

Gradually increase CFL to reduce numerical diffusion:
```
CFL = 0.4  # Test first
CFL = 0.5  # If stable
```

Monitor for numerical instabilities (NaN values, oscillations).

### Priority 5: Adjust Breaking Parameter

```
maxbrsteep = 0.8
```

Delays premature wave breaking.

## Regarding Your Specific Questions

### Q: Are my wave periods too short for XBeach (T < 1s)?

**Answer:** NO - Your wave periods are **perfectly fine** for non-hydrostatic mode.

- The warning about T > 1s is for **surfbeat mode**, not NH mode
- Non-hydrostatic mode explicitly resolves individual waves
- Short periods (T = 0.5-0.9s) are actually **better suited** for NH mode
- NH mode has been validated for laboratory-scale regular waves with T < 1s

### Q: Should I switch to surfbeat mode?

**Answer:** NO - **Stay in non-hydrostatic mode**

Surfbeat mode would be worse because:
- Uses phase-averaged approach (wave groups, not individual waves)
- Designed for T > 1s and infragravity wave generation
- Regular monochromatic waves with T < 1s are outside its intended application
- You would lose the wave-resolving capability you need

**Non-hydrostatic mode is the correct choice for your application.**

### Q: Is my profile setup (starting at wavemaker) the problem?

**Answer:** Unlikely - Your boundary condition setup appears correct.

Your current setup is fine:
- `front = nonh_1d` (correct for NH wave generation)
- `back = abs_1d` (correct absorbing boundary)
- `taper = 5s` (appropriate for T ~ 0.5-0.9s)

The decay issue is primarily **physical dissipation** (bed friction) and **numerical diffusion** (viscosity parameters), not the profile or boundary conditions.

### Potential improvement:

If you have actual measured time series from WG1, consider using:
```
wbctype = ts_nonh
tsspecloc = 1
bcfile = timeseries_wg1.txt
```

This eliminates uncertainties in internal wave generation.

## Implementation Strategy

### Step 1: Test Bed Friction Change Alone

Change ONLY the bed friction file:
```
bedfricfile = bedfricfile_corrected.txt
```

Run the model and compare results. **This should show major improvement.**

### Step 2: Add Viscosity Reduction

If Step 1 improves but doesn't fully solve the problem, add:
```
nuh = 0.001
smag = 0.1
```

### Step 3: Optimize CFL

Once wave heights match better, optimize CFL:
```
CFL = 0.4  # Then test 0.5 if stable
```

### Step 4: Fine-tune Breaking

If waves still break prematurely:
```
maxbrsteep = 0.8  # Or even 0.9 if needed
```

## Files Provided

1. **`params_recommended.txt`** - Complete parameter file with all recommended changes
2. **`bedfricfile_corrected.txt`** - Corrected bed friction coefficients (cf = 0.003 → 0.002)

## Expected Results

After implementing these changes (especially the bed friction correction), you should see:

- **Regular wave fluctuations** maintained throughout the domain
- **Wave heights at WG2, WG3, WG4** much closer to measured values
- **No premature breaking** before the measured breaking point
- **Non-zero wave heights at WG4** (currently seeing essentially zero)
- **Better match** between simulated and measured water level fluctuations

## Additional Diagnostics

If problems persist after these changes, check:

1. **Grid resolution:** Verify you have 20-30 grid points per wavelength in shallow areas
2. **Time step:** Ensure dt is small enough (check XBeach output for actual dt used)
3. **Depth file:** Verify `scan1_zcoord_micp2fine_wholeflume.txt` matches your measured bathymetry
4. **Wave boundary values:** Confirm `Hrms = 0.0468 m` and `Tm01 = 0.4925 s` match WG1 measurements

## References

For XBeach non-hydrostatic mode validation with laboratory waves:
- Smit et al. (2013): "Depth-induced wave breaking in a non-hydrostatic, near-shore wave model"
- XBeach Manual Section on NH mode: https://xbeach.readthedocs.io/en/latest/

## Contact

If you need further assistance after implementing these changes, provide:
1. Comparison plots of wave heights at WG1-WG4 (before/after)
2. XBeach log file output
3. Confirmation of which parameters you changed
