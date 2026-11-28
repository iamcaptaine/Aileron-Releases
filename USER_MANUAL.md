# Aileron User Manual

**Version 0.4.22 - license-enforced beta**

**Airscape Records**  
Made by Matthew Ekeinde AKA Captain E

The current landscape layout uses the slate-and-mint style. It includes a timeline waveform with threshold indicators, a live input frequency spectrum, a single two-endpoint frequency range slider with interior sweep, A/B settings comparison, and a correlation meter with a fading trail.

Aileron is a dynamic stereo panner. It can alternate separate sounds between the left and right speakers, produce continuous tempo-synced movement, or run a threshold-gated LFO.

## Find what you need

- [Quick start](#quick-start-alternate-sounds)
- [Motion modes](#motion-modes)
- [Every knob and control](#controls)
- [Hysteresis example](#understanding-hysteresis)
- [Frequency selection](#process-a-selected-frequency-band)
- [A/B comparison](#compare-settings-with-a-and-b)
- [Reading the displays](#reading-the-display)
- [Activation, trial and transfers](#license-activation-and-transfers)
- [Updates](#checking-for-and-installing-updates)
- [Reset Defaults](#restoring-factory-defaults)

## Manufacturer change in 0.4.10

The manufacturer code is now `Arsc` (exact casing), replacing `InAu`. Avid acceptance and commercial AAX signing are pending. Updating replaces the previous plug-in files. Old AU/AAX sessions may not locate the new identity; VST3 declares compatibility for supporting hosts only. Keep the old installer, back up projects, and export presets or render critical tracks before upgrading. Contact contact@airscaperecords.com if you need help with an older project.

## Installation

The **Airscape-Aileron-0.4.22-macOS-arm64.pkg** development installer includes AU, VST3 and AAX Native for Apple Silicon. Quit your DAW, open the installer, choose the formats you need and follow the prompts. Restart your DAW and rescan its plugins if necessary. The installer is unsigned and not notarized. The AAX build is unsigned for Avid/PACE and requires Pro Tools Developer; it has not been validated in Pro Tools. AU and VST3 DAW validation also remains outstanding.

The **Airscape-Aileron-0.4.22-Windows-x64.exe** installer supports 64-bit Windows 10 and later. Close your DAW, choose VST3, AAX Native or both, then restart and rescan the DAW. Plug-ins install under Program Files\Common Files. Use Windows Settings > Apps > Aileron to uninstall. The development EXE is unsigned, and AAX testing requires Pro Tools Developer.

### Installation locations

| Platform / format | System-wide destination |
| --- | --- |
| macOS AU | `/Library/Audio/Plug-Ins/Components/Aileron.component` |
| macOS VST3 | `/Library/Audio/Plug-Ins/VST3/Aileron.vst3` |
| macOS AAX | `/Library/Application Support/Avid/Audio/Plug-Ins/Aileron.aaxplugin` |
| Windows VST3 | `C:\Program Files\Common Files\VST3\Aileron.vst3` |
| Windows AAX | `C:\Program Files\Common Files\Avid\Audio\Plug-Ins\Aileron.aaxplugin` |

Installation requests administrator authorization. The macOS build is Apple Silicon only; it does not include an Intel binary. The installers do not install the standalone app. The Windows build workflow tests selective installation, upgrade, payload integrity and uninstall on a disposable runner before uploading the installer. Real DAW qualification remains separate from those checks.

## Quick start: alternate sounds

1. Insert Aileron on a track with a stereo output and play audio through it.
2. Under **Motion**, select **Burst**.
3. Choose **Start left** or **Start right**. Before the first trigger, pan is centered.
4. Watch the waveform in the **central waveform display** and set **Threshold** so the white detector envelope crosses the amber opening line on the sounds you want to count. Adjust **Release** so the envelope falls below the faint closing line between separate sounds.
5. Set **Bursts per side**. At 1, each detected sound alternates sides. At 2, the pattern is left, left, right, right when starting left.
6. Choose **Hold last side** or **Return to center** for the gaps between sounds.
7. Adjust **Depth**, **Mix**, and **Pan smoothing** to taste.

Start with the 30.0 ms smoothing default. Turn it down toward Instant for sharp switches or increase it for slower glides.

A burst is an envelope crossing, not a detected word or note. A sustained sound stays on one side. To count the next burst, the envelope must fall below the closing threshold before crossing the opening threshold again. Playback preparation or re-entering trigger mode resets the sequence.

## Motion modes

### Burst

Each qualifying burst stays on its assigned side. **Bursts per side** ranges from 1 to 20. Changing the count during a sound takes effect on the next trigger, using the number already counted on the current side. The starting-side setting controls the first burst after preparation or re-entering the mode.

**Hold last side** leaves the pan at its last position between bursts. **Return to center** moves it toward center until the next burst; the burst count is retained.

### Auto-pan (beats)

This mode moves continuously, regardless of the threshold or sidechain. Choose a waveform and a cycle length from **1/64T to 32 bars**, including straight, triplet (**T**) and dotted (**D**) note values. A cycle is one complete left–right–left movement. Bar lengths assume 4/4.

At 120 BPM, a 1/4-note cycle lasts half a second and 32 bars last 64 seconds. The plugin follows the host tempo; if none is available it starts at 120 BPM or uses the last valid tempo received. Phase runs continuously and does not snap to the song grid. This is panning, not a separate volume tremolo.

The selector includes straight, triplet and dotted versions of **1/64, 1/32, 1/16, 1/8, 1/4, 1/2 and 1/1**, followed by **2, 4, 8, 16 and 32 bars**. Triplets last two-thirds of the corresponding straight value; dotted values last one and a half times the straight value. The menu is ordered from fastest to slowest. New instances start at **1/4**.

Earlier sessions preserve their previous beat selection and automation. Choosing a value in the expanded selector switches that session to the new note-rate controls.

### Quick start: continuous auto-pan

1. Select **Auto-pan (beats)** in Motion.
2. Choose **Sine** and **1/4** for a rounded cycle following your DAW tempo.
3. Set Depth and Mix to determine the movement and processed blend.
4. Adjust Pan smoothing from the 30.0 ms default. Fast divisions may need lower smoothing to retain strong movement.
5. For a slow sweep, choose 8, 16 or 32 bars. These lengths assume four beats per bar.

### Timed LFO

The original timed mode oscillates while the detector is open. Choose a waveform, set **Rate** in hertz, or enable **Tempo sync** and select a division. The LFO phase continues while the gate is closed, provided the host keeps processing the plugin. In sync mode, Rate is inactive. Tempo sync now offers the same **1/64T–32 bars** range as Auto-pan, including straight, triplet and dotted notes. Bars assume 4/4. Enable Tempo sync to use these musical rates; leave it off for the free Hz Rate knob. Existing sessions preserve their earlier division and automation until a new cycle is selected.

## Look-ahead: prepare the pan before each burst

**Look-ahead** delays the audible signal by 0–50 ms while the detector reads the incoming signal immediately. This gives the pan time to move before the burst reaches the output. Try 10–20 ms with a short detector Attack, then adjust Pan smoothing to taste. A long Attack can consume the available head start; a long smoothing time may require more look-ahead to reach the intended side.

The default is **0 ms (off)**. The dry mix and plugin bypass use the same delay, and Aileron reports its latency to the DAW for compensation. Monitor through a host that supports plugin delay compensation. Adjust Look-ahead while playback is stopped: changing the delay during audio can produce a discontinuity, and host compensation updates may vary.

External sidechain detection also reads ahead of the delayed main signal. This is a fixed preview interval, not a prediction of future audio. In Return to center mode the detector also sees the end of a burst early; use Hold last side when you want the full delayed tail to retain its side. Continuous auto-pan has no threshold-triggered entry to anticipate, although the audio delay still applies.

## Controls

| Control | What it does |
| --- | --- |
| Threshold | Level at which the detector opens. Lower values make quieter sounds trigger it. |
| Hysteresis | The detector closes below Threshold minus this amount. Helps prevent repeated triggers around the threshold. |
| Attack | How quickly the detector responds to rising level. |
| Release | How quickly the detector falls after the sound gets quieter. Shorter releases separate closer bursts; longer releases can group them. |
| Rate | Free-running LFO speed in hertz, used in Timed LFO when Tempo sync is off. |
| Depth | Maximum amount of pan movement. Zero leaves the pan centered. |
| Mix | Blends dry and processed audio. With Selected band only on, it controls the processed contribution within that band; the remainder stays dry. |
| Width | Controls input-channel separation in True stereo mode; disabled in balance mode. |
| Look-ahead | Advances detection relative to audible output by delaying audio 0–50 ms. Default 0 ms. Adds reported plugin latency. |
| Pan smoothing | Glide from **Instant** to 2,000 ms. Lower values make sharp transitions; higher values make slower glides. The default for new instances is 30.0 ms. |
| External sidechain | Uses a host-routed sidechain as the detector source in trigger and gated modes. The sidechain is not sent to the output. |
| True stereo | Processes the left and right inputs as separate sources that can move across the stereo field. Off uses stereo balance. |
| Bypass | Ramps to the dry signal while the plugin continues its internal processing when the host calls it. |
| Selected band | Applies panning to one selected frequency band. Off restores full-range processing. |
| Frequency range | One logarithmic slider: left endpoint sets the low edge, right endpoint sets the high edge, from 20 Hz to 20 kHz. |
| A / B | Switches between two independent sets of processing settings. |
| COPY A → B / COPY B → A | Replaces the other comparison slot with the current settings. |

Pan smoothing is an exponential time constant: about 63% of the remaining distance is covered after the displayed time, and about 95% after three times that value. Long smoothing values can prevent short sounds from reaching the full selected depth. The original fixed waveform smoothing still applies in LFO modes.

At the **30.0 ms** default, a transition covers about 95% of its distance in **90 ms**. Smoothing affects transitions between sides and returns to center; it does not change the trigger threshold or burst count. Saved settings take priority over the new-instance default.

### Understanding hysteresis

Hysteresis is the gap between the level that starts a burst and the level that ends it. The detector opens when its envelope reaches **Threshold**, but only closes when the envelope falls below **Threshold minus Hysteresis**. After it closes, the next rise to Threshold can count as a new burst.

For example, with **Threshold at −18 dBFS** and **Hysteresis at 3 dB**, the detector opens at **−18 dBFS** and closes only below **−21 dBFS**. A dip to −19 or −20 dBFS keeps the same burst active; a dip below −21 dBFS rearms the detector once the envelope reaches that level. The next rise to −18 dBFS starts a new burst.

Increasing Hysteresis helps keep a wavering sound together and reduces repeated triggers near the threshold. Lowering it makes smaller dips more likely to separate bursts. **Release** also matters: it controls how quickly the detector envelope falls, so a brief dip in the audio may not last long enough to close the detector.

Hysteresis affects **Burst** and the threshold gate in **Timed LFO**. Continuous **Auto-pan (beats)** ignores it.

### Waveforms

- **Sine:** smooth, rounded movement.
- **Triangle:** a linear rising and falling shape, subject to smoothing.
- **Square:** alternates between sides with smoothed waveform transitions.
- **Sample & Hold:** chooses a new random position on each LFO cycle.

## Stereo and sidechain routing

Aileron supports mono-to-stereo and stereo-to-stereo processing. Insert it where the DAW provides a stereo output. In balance mode, panning attenuates the opposite input channel. Enable **True stereo** to reposition both input channels and use Width. Mono input uses a single equal-power panner.

For external triggering, enable **External sidechain**, activate the plugin's sidechain input in your DAW, and route a trigger track to it. DAWs expose this routing differently. A missing sidechain is treated as silence. External triggering does not control the continuous Auto-pan mode.

Combining correlated stereo channels can raise peaks; leave headroom and watch the output meters.

## Process a selected frequency band

Enable **Selected band** in the central Frequency range panel. Drag the **left endpoint** of the single range slider to set the lowest processed frequency, then drag the **right endpoint** to set the highest. The readouts above the handles show both limits. The mint segment and shaded region show the selected band. The endpoints cannot cross during a drag.

The control spans **20 Hz–20 kHz** on a logarithmic scale. Equal distances represent frequency ratios: the reference marks at 20 Hz, 200 Hz, 2 kHz and 20 kHz are equally spaced. For example:

- **200 Hz–5 kHz:** move mainly the midrange while leaving the lows and highest frequencies mostly dry.
- **5 kHz–20 kHz:** apply motion mainly to the high-frequency content.
- Turn **Selected band off** to restore the original full-range effect. The spectrum continues updating, but the range handles stop responding until the option is enabled again.

This is a processing range, not an EQ gain control. Frequencies outside the band remain on a complementary dry path. The filters have smooth 12 dB/octave slopes, so audio around each edge receives part of the effect. The boundaries are not brick-wall cuts. Cutoff and enable changes are smoothed, and selecting a band adds no audio latency. At Mix 0 or bypass, the band and its remainder reconstruct the dry signal, with the existing mono center law and any Look-ahead delay still applying.

Band selection affects the audio being panned, **not the trigger source**. The detector still follows the full main input or the external sidechain. A low-frequency sound can therefore trigger motion in a selected high-frequency band. Depth, Mix, Width and the selected motion mode still apply. Listen around the filter edges when using extreme settings.

The effective upper edge is limited to **45% of the current sample rate**. At 44.1 kHz this is approximately 19.85 kHz, even if the readout is set to 20 kHz. Existing sessions keep their saved band parameters. Older sessions without band settings load with full-range processing; older crossed edge values display in frequency order without rewriting their automation.

## Compare settings with A and B

**A** and **B** store independent sets of all processing controls, including motion, detector, band, Mix and Bypass settings.

1. Select **A** and adjust the sound.
2. Select **B**. On its first visit, the other slot starts as a copy of the current settings.
3. Change B, then select A and B to compare.
4. Use **COPY A → B** while A is selected, or **COPY B → A** while B is selected, to overwrite the other slot with the current settings.

Both slots and the selected slot survive closing the editor and saving/reopening a DAW session. A/B switching keeps the live oscillator and detector history running, using the existing parameter smoothing. It does not match perceived loudness or capture audio for playback. Switching sends parameter changes to the DAW, so it can be recorded when host automation is writing. License activation is shared and is not an A/B setting.

## Reading the display

Use **BURST**, **AUTO-PAN** or **TIMED LFO** in the top Motion strip to choose the mode and configure its trigger or timing fields. **True stereo** is at the right of that strip; **External sidechain** is below the Detector controls. The left **Detector** column groups Threshold, Hysteresis, Attack, Release and Look-ahead. The right **Pan & output** column groups Depth, Pan smoothing, Rate, Width and Mix. The central panels show the waveform or vectorscope above the spectrum and selected range. Correlation, pan position and stereo output meters occupy the bottom strip.

### Timeline waveform and threshold

The **central waveform display** opens with a six-second, time-domain waveform. It shows the incoming sound over time, rather than a frequency spectrum. Both channels' positive and negative peaks are retained in short display intervals so brief transients remain visible.

- **Amber lines:** the opening threshold, shown above and below the waveform center line.
- **Faint amber lines:** the closing threshold, equal to Threshold minus Hysteresis.
- **White trace:** the detector envelope after Attack and Release. This envelope controls triggering; a raw waveform peak crossing the line does not always mean the detector has opened.

The waveform and threshold share a fixed full-scale amplitude reference. The display does not automatically enlarge quiet signals, and peaks beyond full scale are clipped visually only. Adjust Threshold while watching the envelope to understand which sounds trigger the effect.

With **External sidechain** on, the waveform follows that sidechain. It shows incoming audio before Look-ahead delay. **Auto-pan (beats)** ignores the threshold and detector even though the display remains visible. Select **Vectorscope** to switch to the stereo view, or **Waveform** to return.

### Input spectrum and frequency range

The lower panel overlays a live **input frequency spectrum** behind the range slider. From left to right, it shows low to high frequencies on the same logarithmic scale as the handles. Taller peaks indicate stronger signal content at those frequencies. The selected band is mint; frequencies outside it are gray. The vertical selection lines connect each endpoint to its frequency readout.

The spectrum always follows the **main input**, before band filtering, panning, Mix and Look-ahead delay. It does not switch to the external sidechain. For example, if a sidechain kick triggers panning on a vocal track, the waveform above can show the kick while the spectrum below shows the vocal.

The level scale spans **0 to −96 dBFS**, with reference lines at −24, −48 and −72 dBFS. Both channels contribute by power, so opposite-polarity stereo content remains visible. The graph updates while the editor is open, and displayed peaks decay smoothly. If the DAW stops processing, the trace fades instead of holding stale signal indefinitely.

Frequency detail depends on sample rate and the analysis window: the 8,192-sample Hann-window analysis has about 5.9 Hz/bin resolution at 48 kHz and 11.7 Hz/bin at 96 kHz. Narrow tones appear broadened by the window. This display helps locate frequency content; it is not a loudness, true-peak or precision measurement tool. Analysis adds no audio latency.

### Stereo, pan and output readings

The **vectorscope** plots the output in mid/side form. The **Pan position** readout and horizontal marker show the current position between left, center and right. **Correlation** indicates how closely the left and right outputs move together: positive values indicate greater similarity, negative values indicate opposing polarity, and silence returns to the neutral baseline. The output meters show channel peaks. These displays are visual guides, not calibrated loudness or true-peak meters.

### Window size and About

The default window is **1008 × 630**. Drag the bottom-right corner to resize proportionally from **864 × 540** to **2160 × 1350**. Hosts may expose resizing differently. Select **About** to see the version and credits; close it with **Close** or Escape.

## Troubleshooting

**The sides do not alternate:** Confirm Burst is selected, lower Threshold, and check that the detector closes between sounds. Shorten Release or reduce Hysteresis if neighboring bursts are grouped. Check Bursts / side and the selected sidechain source.

**A sound flips unexpectedly:** Increase Hysteresis or Release to reduce retriggering from brief dips. Raise Threshold if background noise triggers the detector.

**Auto-pan is too subtle:** Increase Depth and Mix. Reduce Pan smoothing, particularly at fast cycle lengths. In balance mode, stereo content already isolated to one channel may behave differently from a mono source.

**Width is disabled:** Turn on True stereo. Width does not apply in balance mode or change mono panning.

**Smoothing still shows 5 ms after updating:** Existing instances and saved sessions restore their saved values. Set the knob to 30.0 ms for that instance; the new default applies to freshly created instances unless your DAW loads a saved default preset.

**Tempo movement seems out of sync with the song:** Confirm the DAW supplies tempo. The rate follows tempo, but phase is not locked to the song position. Bar lengths are fixed to four beats.

**The plugin is missing from the DAW:** Confirm the correct format is installed, restart and rescan the DAW, and check platform compatibility: Apple Silicon on macOS, or an x64-compatible Windows 10 or later environment. The included AAX build requires Pro Tools Developer and is not a signed release build.


**The range handles do not respond:** Enable Selected band only. When it is off, the spectrum remains visible but the handles do not change the band.

**A sound outside the band still triggers movement:** Band selection controls which audio is panned. The detector still hears the full input or the external sidechain.

**The waveform and spectrum show different signals:** With External sidechain on, the waveform follows the sidechain, while the spectrum always follows the main track input. This is expected.

**The waveform crosses Threshold but no burst starts:** Watch the white detector envelope. Attack can prevent a short raw peak from opening the detector. Another burst also requires the envelope to fall below Threshold minus Hysteresis before it can reopen.

**The spectrum is blank or fading:** Play audio through the main input and confirm the DAW is processing the plugin. External-sidechain audio alone does not populate the spectrum. After opening the editor or changing sample rate, allow a short interval for the analysis window to fill.

**A/B switching changes the sound more than expected:** Each slot includes all processing controls, including Mix, Bypass and the selected band. Compare those settings and remember that A/B does not match loudness.

## License activation and transfers

Open **License** in the header, paste your Aileron license key, and select **Activate**. A standard commercial license permits two active devices. The separately issued early-access campaign licenses permit one active computer and require Aileron 0.4.22 or later. The campaign does not change existing or purchased two-device licenses. The plug-in formats and DAW instances share an activation within the same computer login.

To move an activation, select **Deactivate this device** on the old computer while online. After confirmation, activate the license on the new computer. If the old computer is unavailable, contact Airscape Records to release its activation. Deleting the plug-in does not release a slot.

When the editor opens without authorization, the License pop-up offers **Activate** with your license key or **Start 15-day trial**. The trial starts only when selected, enables all features for 15 consecutive calendar days, and shows days remaining. Its machine-bound record is shared across formats within your computer login and persists across restarts. It does not consume a license device slot. After expiry, activate a license to continue processing; closing the pop-up leaves dry audio. The local trial works offline and checks for clock rollback. Local record deletion or modification can defeat this basic trial protection; it is not a server-issued entitlement.

Outside the trial, this beta requires an assigned license key before the effect operates. Standard licenses have no expiry and support two active devices; the separate early-access campaign licenses have no expiry and support one active computer. Open License, paste the key and select Activate. In 0.4.21, connect once to activate a Polar license. Aileron then saves device-bound offline authorization in macOS Keychain or Windows protected storage. You can reopen your DAW projects and use the effect without internet access, with no offline time limit. Aileron checks Polar in the background at startup and approximately once per minute while running. Connection failures, rate limits and temporary server failures preserve offline access. A confirmed rejected or revoked activation removes offline authorization and returns the effect to dry audio with the existing Look-ahead latency; it does not mute the track. Revocations or transfers performed through the customer portal take effect on an offline computer when it next successfully contacts Polar. First activation and confirmed deactivation require internet access. Existing Polar activations from older versions need one successful online verification in 0.4.21 to prepare offline use. The License panel confirms “Ready for offline use” or reports a secure-storage problem; if access to Keychain/protected storage is denied, online validation remains necessary. Legacy Lemon Squeezy keys retain their online-validation behavior. The footer shows Activation required - dry audio while unlicensed. License details are stored outside DAW projects and presets.

Version 0.4.15 supports Airscape Records licenses issued through Polar. These keys start with AILERON- and have no expiry. Standard purchased licenses allow two active devices; campaign licenses allow one. Your purchase email identifies your customer record; paste the license key into Aileron to activate. You can retrieve your key and release a lost computer activation at https://polar.sh/airscape-records/portal using that email address. Keep keys private.

Existing Lemon Squeezy beta keys and saved activations remain supported while that service is available. To change to a Polar key, first choose Deactivate this device with your old key, then enter the new key. Installing an update does not convert or replace your key. Contact contact@airscaperecords.com if the old service or computer is unavailable. Polar keys require version 0.4.15 or later; older installers cannot activate them. Earlier unrestricted binaries are not retroactively locked.


### If the license service cannot be reached

Check your internet connection and any app-specific firewall rules. Aileron connects to **api.polar.sh** for Polar licenses and **api.lemonsqueezy.com** for existing Lemon Squeezy beta keys, using HTTPS. In Little Snitch or a similar firewall, check Aileron's rule for the standalone app, or your DAW's rule when using the plug-in. After correcting a blocked rule, close and reopen Aileron or the DAW, then retry **Activate** or **Deactivate this device**.


## Correlation meter and band sweep (0.4.9)

The correlation meter runs from -1 (red, opposite polarity), through 0 (yellow, uncorrelated), to +1 (green, matching channels). The white marker and numeric readout show the current correlation; this replaces the correlation history graph.

With Selected band enabled, drag inside the highlighted frequency band to sweep both edges together. The band retains its width in octaves (the high/low frequency ratio) and stops as a whole at 20 Hz or 20 kHz. Drag directly on either endpoint to resize that edge. Band movement writes both existing automation parameters and remains compatible with A/B recall.


## Correlation meter response (0.4.9)

The white correlation marker and numeric reading use a medium 300 ms smoothing time constant (about 0.7 seconds to move 90% toward a new reading). A yellow band follows recently visited positions and fades over approximately one second. After signal stops, the reading returns smoothly toward zero and the old trail clears. These display changes do not alter audio processing, waveform timing or output-meter ballistics.


## Display-synchronized animation (0.4.9)

The correlation meter, waveform and vectorscope update using display-refresh callbacks for the plugin window. Actual refresh depends on the display, operating system and host; high-refresh displays can provide more frequent animation without selecting a fixed FPS setting. This is not a guarantee of 120 fps in every DAW. The meter keeps its 300 ms response and one-second trail. Output-meter decay remains time based, and spectrum analysis and controls run separately at approximately 30 Hz. A 30 Hz animation fallback is used when display callbacks are unavailable. Hosted editors pause analysis and animation while hidden. Physical multi-display/ProMotion and DAW performance qualification remain necessary.


## Checking for and installing updates

The update checker is available in Aileron 0.4.11 and later. If you have an older
version, first install the latest version from the
[official downloads page](https://github.com/iamcaptaine/Aileron-Releases/releases).
No GitHub account is needed.

Aileron checks for updates when its runtime launches. Instances sharing the same
loaded module share one check. When a newer version is available, the editor shows
a dismissible prompt. Choose **View Update** to open About and review/download it,
or **Not Now** to continue. Checks do not download or install anything automatically.
In **About**, turn off **Automatically check for updates** to disable launch checks
and prompts. This preference is saved on this computer; manual checks remain available.

1. Open Aileron in your music software and click **About** to see the result.
2. To refresh the result or retry a failed launch check, click **Check for Updates**.
3. If a newer version is available, read its release notes before continuing.
4. Click **Download Update**. Wait for Aileron to finish downloading and verifying
   the installer. It is saved directly in your Downloads folder. An existing file
   is preserved and a numbered name is used if necessary. This does not start installation.
5. Click **Show Installer**. The downloaded file appears in Finder on Mac or File
   Explorer on Windows. Keep that folder open.
6. Save your project. Quit every music application that is using Aileron.
7. Open the downloaded installer yourself and follow its prompts. Select the
   plug-in formats you use and approve the normal system installation prompts.
8. Reopen your music software. Rescan its plug-ins if Aileron is not listed.
9. Open Aileron's **About** panel to confirm the installed version. You can then
   delete the installer from Downloads.

### Which format should I install?

- **Logic Pro:** Audio Units on Mac.
- **Other music software:** VST3, if supported by your application.
- **Pro Tools:** AAX. The current beta requires Pro Tools Developer until
  commercial signing is complete.

You can select more than one format if you use several music applications. Mac
installers currently support Apple Silicon only. Windows installers are for
64-bit Windows 10 and later.

### If no update appears or a download fails

If Aileron says you are up to date, no installation is needed. If a check fails,
check your internet connection and try again later. GitHub may temporarily limit
requests. If a download fails verification, do not install that incomplete file;
click **Download Update** again. You can also use the official downloads page.

If you still see the old version after installation, quit all music applications,
reopen your host and rescan its plug-ins. Make sure you installed the format your
host uses. Keep a backup of important projects and the previous installer before
upgrading. Read the manufacturer-change section before updating an installation
older than 0.4.10, because older AU and AAX projects may need their original build.

### Your control and privacy

Launch checks can show an available-update prompt and can be disabled in About. You can also check manually. Nothing is
downloaded without choosing **Download Update**. Updates never install
automatically, and downloading does not
replace the installed plug-in. You choose when to close your music software and
run the installer. Updating does not activate or deactivate your license.

No license key, email, audio or project data is sent when checking for updates.
GitHub receives the normal internet request, including your IP address. Downloads
are checked against GitHub's published file size and SHA-256 checksum before they
are offered for installation. This check does not replace publisher signing.
Unsigned beta, Apple notarization and AAX signing limitations still apply.

In version 0.4.20 and later, verified installers are saved directly in **Downloads**.
Earlier versions use the application-data folder under
`Airscape Records/Aileron/Updates`. Use **Show Installer** to reveal the downloaded file.


License controls: Activate is shown when the license is inactive; Deactivate this device is shown when activated. Activate and the trial button are hidden when activated; Deactivate is hidden when inactive. Visible actions are disabled during a license request. Open License inside Aileron to manage this computer's activation.


### Restoring factory defaults

Click **Reset Defaults** in Aileron's top bar to reset all parameters in the current A/B slot. The other comparison slot, license and update preference are preserved. Hosts that expose the factory program also list **Default** (in Logic: the plug-in Settings menu > AU Presets > Default). Logic's separate **Recall Default** command recalls a user default created with **Save As Default**; it can remain unavailable until a user default is saved.

When licensed, the License panel hides the license-key entry field and its label, Activate and Start 15-day trial, and shows Deactivate this device. When unlicensed, it hides Deactivate and shows activation/trial controls. Visible actions are disabled while a licensing request runs.
