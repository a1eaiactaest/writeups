> # Revision  
> ## Points: 200 
> > ... They aren’t necessarily obvious but are helpful to know.  
> http://git.ritsec.club:7000/Revision.git/
>  
> Author: ~knif3

**This challenge is partly a harder part of `Blob` and `1597` challenges**

## Solution

In description we are prompted with another git repository. After cloning it we see a lot of html files. It basically is a  [www.ritsec.club](https://www.ritsec.club) source code files, as `README.md` states.  


There's no such a file like `flag.txt` in whole repo.   
Maybe we should search for commits that were containing the flag? By "containing" I mean have something to do with eg. modifying, adding, deleting.

```
git log --all --full-history -- flag.txt
```
Output we get:

```
commit 88aaf373f80263e14713efea263ac99550711322
Author: knif3 <knif3@mail.rit.edu>
Date:   Wed Feb 6 18:52:23 2019 -0500

    TXkgZGVzaWduIGlzIGNvbXBsZXRlIQo=

commit 68733d819366b78225df3525017876319b96b1a5
Author: knif3 <knif3@mail.rit.edu>
Date:   Wed Feb 6 18:52:23 2019 -0500

    N2Q5ZDI1ZjcxY2I4YTVhYmE4NjIwMjU0MGEyMGQ0MDUK

commit b1a0dcb3a818e952c58f2758b5cd5896945826b2
Author: knif3 <knif3@mail.rit.edu>
Date:   Wed Feb 6 18:52:23 2019 -0500

    ZjRkNWQwYzA2NzFiZTIwMmJjMjQxODA3YzI0M2U4MGIK

commit 4a2893a847f5cf69aafd4f64379013635663156b
Author: knif3 <knif3@mail.rit.edu>
Date:   Wed Feb 6 18:52:23 2019 -0500

    NjliNjQ2MjNmODZkZWYxNmNlMTdkNDU0YjhiZTQxYWUK


[...]
```

Commit descriptions are in base and contain nothing special.

Let's get back to the specific commit.

```
$ git reset --hard 88aaf373f80263e14713efea263ac99550711322
HEAD is now at 88aaf37 TXkgZGVzaWduIGlzIGNvbXBsZXRlIQo=
$ cat flag.txt
cat: flag.txt: No such file or directory
```

Nothing here, let's check another one.

```
$ git reset --hard 68733d819366b78225df3525017876319b96b1a5
HEAD is now at 68733d8 N2Q5ZDI1ZjcxY2I4YTVhYmE4NjIwMjU0MGEyMGQ0MDUK
$ cat flag.txt
}
```

It looks like it's last char of the flag.

I wrote a script that gets back to the commit, reads `flag.txt` content and saves it.


```python
#!/usr/bin/env python3
import os
import time

commits = '''
68733d819366b78225df3525017876319b96b1a5
b1a0dcb3a818e952c58f2758b5cd5896945826b2
4a2893a847f5cf69aafd4f64379013635663156b
496362794582e85dd7b1db0cad5f20970f39e1fa
0f40e6ec4446279a8334f0f312f5ade439bf7f20
8e8ce11ebb15f92f9de3aaacf87874bc4288447e
60f4e46c34647c9529da9e9e518b58b719adbe1b
ee3d68b9b6e5330c67ad1b3dd93275258b849c0a
ebd4f62b0295d5e17af0cab3ad630028e4d13133
53e3f77a3e21abe8ee7eca6d5bd69caf281b72d1
184914866cb2536d7d3d22ef1e1a521df8e22086
9d6d71417e4ef9241e72ce3f796df0242ebb71a8
eae1814581c32f536abd349d7d2deed2965caa04
a2e1f5ba01aaa238d137e62270fb7f8ddf06e10a
82b7ac89d4ac53045fdd21d6ede02d4d6fc4cdab
1b07dd891d41e082cfa28ebe48345d7970dfa3f8
c410e16cbba03b29495a2f2d6b10081c701e628c
a4ee01c27519aa2c160c167eb2ff569aa31582cf
2e1c2e0cd405c5d3a3302b165705808d86e4966a
6fcc88671072e484ae06ecae35d639e00c6a3c86
2a712b002849b5ee44c2a139f5f99df049327f87
58e54d74610cce1c6c777e305bad39ace9d1b6c8
d8ddbff68296aa033dea7635924c02b296e00e94
432c9eeaed907425ea7e0469c50fbe2ca298d243
f108cdb5227a24fa75d9774fb572fd9f30996950
9fe2c0ed9d9be950e1fe6a74d0b51039053d2b5a
50f69d69ece32e4217baf9d5581bbb53d313ddbb
6f621dba0a3c2cafc863f0acfbb291cac8cb26fa
48297e0c0b6c3d739944cc5cda73245e767ee4c7
fca179e4bc0b1621365278cb9562dd65c4df191c
8aeb0759342ffafc84a812c0699e9ea8caa3fd36
3abc63e80c3cb5fdb8d0d422b2013724ee675417
577232034921096b3fe59d9826fbb42a31ba3f96
764794b7567ee2b468eedba22d3bc2eed9795f52
f23c83288b5f594a595aef47a7c5cb37213f31dd
331c43d1166915dd6e10994d531de9f3d986d616
2092ddb909befa6ceee7449e4ce9433ba8bc8d57
2a769ddc9f4bfdfcdce753068497ca251996f7db'''

commits= commits.split()

ret = []
#os.popen('cd Revison')
for i in range(0,38):
  commit = commits[i]
  os.system('git reset --hard %s'% commit)
  #os.popen('cat flag.txt')
  ret.append(os.popen('cat flag.txt').read())


print('Returning to the newest commit..')
time.sleep(1)
os.system('git pull')
ret = [x.strip('\n') for x in ret]
print(''.join(ret[::-1]))
```

Let's put the script to the repo and run it.

And we get a flag: `RS{I_h0p3_y0u_scr1pted_th0s3_git_c0ms}`
