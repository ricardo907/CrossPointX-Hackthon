STILL RENTING

STILL RENTING is a reverse-UX prototype for a self-service camera rental return. Once the locker records the equipment as returned, the rental stays open while the user re-enters known facts, repeats checks, and gets an approved request sent back for rework. The project looks at how an interface can make someone feel close to finishing while repeatedly asking them to do more.

## Try it or watch the demo

| File | What it contains |
| --- | --- |
| [English source and runnable build](./STILL_RENTING_V1.5.1_English.zip) | The complete V1.5.1 source, prebuilt frontend, documentation, and tests. |
| [Submission PDF and walkthrough](./STILL_RENTING_Submission_Links_CrossPointX.pdf) | Link verification notes and a 3-minute, 38-second English walkthrough. Download the PDF and open its **Attachments** panel in a desktop PDF reader to play the MP4. |

The walkthrough shows the full return, all four support transfers, an expired CAPTCHA attempt, and a downloaded receipt that matches the server record. It uses the local reason reviewer and an isolated test code for the successful CAPTCHA retry.

## Run locally

1. Install **Node.js 24 or later** and extract the English ZIP.
2. On Windows, double-click `Start_Windows.bat` inside the extracted `STILL_RENTING` folder. On any supported platform, run this command from that folder.

   ```sh
   node portable/server.mjs --open
   ```

3. Open `http://127.0.0.1:8787` if the browser does not open automatically, then select **Start return demo**.

The prebuilt release runs offline without installing npm dependencies. Keep the server terminal open while using it. Press **Ctrl+C** to stop it.

## What happens in a return

1. **Return the kit.** Put the camera body, lens cap, and battery in the simulated locker and close the door. The server records the return time and freezes the equipment fee.
2. **Prove it again.** Re-enter five recorded facts, reveal the checks one by one, and repeat four confirmations after selecting a specific reason.
3. **File the request.** Retrieve document codes from separate desks. The first request comes back for a missing certificate. After approval, withdraw, reissue, file, and acknowledge the request. The optional support path can send the conversation through four transfers.
4. **Get past three 100% screens.** The results open a 45-second written reason review, an 18-second reversed locker transcription, and a 20-second CAPTCHA. The timed fields block paste. Expiry discards the current entry and sends the user back to the preceding progress screen.
5. **Close the rental.** Explicitly confirm settlement, download the JSON receipt, and check its verification code against the server. The displayed waiting estimate is reversed in full.

All equipment and charges are simulated.

## Implementation

- **Interface** · React and TypeScript, built with Vite for the portable release.
- **Server and data** · A Node.js HTTP server and SQLite. The shared business service controls deadlines, validates state transitions, and writes one receipt per rental. Closure requests can be retried with the same operation ID.
- **Reason review** · Local rules run by default. Set `OPENAI_API_KEY` in the server environment to enable optional model review, with local rules as the fallback.

The main interface is in `app/`, business rules and API handlers are in `server/`, the local runtime is in `portable/`, and automated checks are in `tests/`.

## Build and test

Run these commands from the extracted source directory.

```sh
npm ci
npm test
npm run typecheck
npm run portable:build
```

V1.5.1 passed 70 Node tests and 15 HTTP smoke checks against a freshly extracted portable release. The checks cover the return flow, settlement, receipt download and verification, and recovery after a restart.
