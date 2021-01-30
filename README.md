The program RC3Mod is a modification of the original RC4 program, in an attempt to overcome its vulnerability.
<p>The principle of th modification is to insert a random scrambler into the algorithm, which modifies the S array and creates a hash table to transform the plain text before it is encryptd.  This will result in a different cipher text being produced with each encryption, even if the password and the plain text remains the same.
<p>The program is divided into 3 classes.
<p>The first is ClassRndomGen, a portable random number generator Ran3, copied from the textbook Numerical Recipes (see reference).  The class produces the same results from the same random seed, independent of the operating system of the computers used.  The program is modified so it can scramble an array in a deterministic manner according the the random seed used.
<p>The second is ClassRC4, which is really the originsl standard RC4 algorithm.  I copied it from https://gist.github.com/farhadi/2185197, but modified it so it can be incorporated into the new program.  The modification separates the preparation of the S array from its use, allowing further changes to the S array from an external source, and allowing the S array to be modified by an array of bytes of any length.
<p>The third is ClassRC4Mod, which is the modified part but uses the other two classes in the following manner
<ol>
  <li>Step 1. It initialises the RC4 S array using the password</li>
  <li>Step 2. It uses system resources and generate 2 random bytes, encrypted these bytes by RC4 as the first 2 bytes (4 hex chrs) of the cipher text.  The two bytes are then combined to form an integer, which is used to seed the random number generator, to further change the S array, and the created the hash table.
  <li>Step 3. The two bytes are then merged into a single scrambler byte, which is used to obfuscate each byte from the plain text before it is hashed, then encrypted.  The hashed byte is then used as the scrambling byte for the next byte of the plain text.
</ol>
In deciphering, the process is reversed.
<ol>
  <li>Step 1. The S array is promed by the password.  The first 2 bytes (4 hex chrs) of the cipher text is then deciphered into the original random bytes.  This is used to modify the S array, create the unhash table, and reconstitute the scrambler byte.
  <li>Step 2. The remainder of the cipher text is then deciphered in the reverse order of encryption, and produce the cipher text.
</ol>
The algorithm is described with more details in http://www.statstodo.com/RC4Mod_Exp.php accompanied by flow charts demonstrating the algorithm.
Readers are invited to critically examine the algorithm, and attempt to break the encryption.   I would be grateful for feedback
<p>Ref:
<p>Press WH, Flannery BP,Teukolsky SA, Vetterling WT (1994). Numerical recipes in Pascal Ed. 1, IBSN 0-521-37516-9.  p.221 for Ran3 random number generator
<p>farhadi / rc4.js in Instantly share code, notes, and snippets. https://gist.github.com/farhadi/2185197
