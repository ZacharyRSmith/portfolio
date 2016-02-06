# Code sample

One of my proudest refactorings is [this PR](https://github.com/HR10Knights/Sage/pull/96), especially:

Before: See anonymous function beginning at [doozy/routes/index.js line 15](https://github.com/ZacharyRSmith/Sage/commit/029f191beef73b715dc71c5bfc0b556502b24ab9#diff-d5ab1e2c4c009ef73d0dc63b1c182343L15).

After: See indexController#createUser beginning at [doozy/controllers/indexController.js line 13](https://github.com/ZacharyRSmith/Sage/commit/029f191beef73b715dc71c5bfc0b556502b24ab9#diff-ce50fdc9bc8511d3b625854a16a1bf43R13).

Relevant tests are in [test/integration/apiauth.js](https://github.com/ZacharyRSmith/Sage/commit/029f191beef73b715dc71c5bfc0b556502b24ab9#diff-7e1a68f2de537b9eb615ac01191e9dfaL1).

While I'm at it, I wrote [this](https://gist.github.com/ZacharyRSmith/343962433a89ef1b454a) to share with my team.  It explains a code pattern I used to help me in the code clean up I linked above.
