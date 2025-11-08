// run a loop of select all on a page (50 messages), delete, wait until it can select again, repeat for X iterations. 
// It's a bit raw but it works and has been helpful for me as as I transition from gmail and clean up years of old mail. 
// 
// run this by pasting the code into the Safari -> Develop -> Show JavaScript Console

(async () => {
  const runs = 10;            // number of runs
  const timeout = 30*1000;    // 30 seconds (in ms)

  async function waitForElement(selectorOrId, isId = false, timeoutMs = timeout) {
    const start = Date.now();
    while (Date.now() - start < timeoutMs) {
      const el = isId
        ? document.getElementById(selectorOrId)
        : document.querySelector(selectorOrId);
      if (el && !el.disabled) return el;
      await new Promise(r => setTimeout(r, 100));
    }
    throw new Error(`Timeout waiting for ${selectorOrId}`);
  }

  console.log(`Starting ${runs} runs... (timeout = ${timeout / 1000}s)`);

  for (let i = 1; i <= runs; i++) {
    console.log(`Run ${i} starting... (timeout = ${timeout / 1000}s)`);

    try {
      const box = await waitForElement('idSelectAll', true);
      if (!box.checked) box.click();
      await new Promise(r => setTimeout(r, 300));

      const trashButton = await waitForElement('[data-testid="toolbar:movetotrash"]');
      trashButton.click();

      console.log(`Run ${i} complete.`);
    } catch (err) {
      console.error(`Run ${i} failed: ${err.message}`);
    }

    // pause before the next run
    await new Promise(r => setTimeout(r, 2000));
  }

  console.log(`All ${runs} runs finished.`);
})();
