/** 
 * TpClub Cheat:
 * 1. Open the website
 * 2. Start any level
 * 3. Open the console and type the command 'allow pasting'
 * 4. Paste the script and press ENTER
 */

// The lower the following values, the faster the autotyper
const minDelay = 120;  // Adjusted minimum delay
const maxDelay = 230;  // Maximum delay stays the same

// Add variables for dynamic variation
const dynamicMinDelay = 90;  // Adjusted dynamic minimum delay
const dynamicMaxDelay = 310;  // Maximum delay stays the same

const mistakeProbability = 0.04; // 4% chance to make a mistake

const keyOverrides = {
  [String.fromCharCode(160)]: ' '    // convert hardspace to normal space
};

function getTargetCharacters() {
  const els = Array.from(document.querySelectorAll('.token span.token_unit'));
  const chrs = els
    .map(el => {
      // get letter to type from each letter DOM element
      if (el.firstChild?.classList?.contains('_enter')) {
        // special case: ENTER
        return '\n';
      }
      let text = el.textContent[0];
      return text;
    })
    .map(c => keyOverrides.hasOwnProperty(c) ? keyOverrides[c] : c); // convert special characters
  return chrs;
}

function recordKey(chr) {
  // send it straight to the internal API
  window.core.record_keydown_time(chr);
}

function sleep(ms) {
  return new Promise(r => setTimeout(r, ms));
}

// New function to introduce varying delays
function getVaryingDelay() {
  // Create a random delay that fluctuates between dynamicMinDelay and dynamicMaxDelay
  const randomFactor = Math.random();  // Will be between 0 and 1
  const sineWaveFactor = Math.sin(randomFactor * Math.PI);  // Sinusoidal variation for smooth randomness
  
  // Calculate a random delay with a fluctuation pattern
  return dynamicMinDelay + (dynamicMaxDelay - dynamicMinDelay) * sineWaveFactor;
}

// Function to simulate the backspace key
function pressBackspace() {
  const event = new KeyboardEvent('keydown', {
    key: 'Backspace',
    code: 'Backspace',
    keyCode: 8,
    which: 8,
    bubbles: true,
    cancelable: true
  });
  document.dispatchEvent(event);
}

async function autoPlay(finish) {
  const chrs = getTargetCharacters();
  for (let i = 0; i < chrs.length - (!finish); ++i) {
    let c = chrs[i];

    // Simulate the decision to make a mistake
    if (Math.random() < mistakeProbability) {
      // Choose a random "wrong" character to type as a mistake
      const wrongChar = String.fromCharCode(Math.floor(Math.random() * 26) + 97); // Random letter (a-z)
      console.log(`Making a mistake: ${wrongChar}`);

      // Type the wrong character
      recordKey(wrongChar);
      await sleep(getVaryingDelay()); // Varying speed

      // Wait for 0.3 seconds before correcting the mistake
      await sleep(300); // Fixed 0.3 seconds delay before correcting the mistake

      // Press backspace to delete the wrong character
      pressBackspace();
      await sleep(getVaryingDelay()); // Varying speed

      // Now type the correct character
      recordKey(c);
      console.log(`Correcting to: ${c}`);
      await sleep(getVaryingDelay()); // Varying speed
    } else {
      // No mistake, type the correct character
      recordKey(c);
      await sleep(getVaryingDelay()); // Varying speed
    }
  }
}

// ############################################################################################################
// Go!
// ############################################################################################################

autoPlay(true);
