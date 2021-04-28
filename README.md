# CONDENSED : CONvert Diffs to fakE vim sessioNs for aSciinema rEcorDing

based on [this lovely, simple cli editor](https://github.com/tdryer/editor), we
take in a diff, figure out which file it edits, and then send a series of
keypresses which make it look like we're typing interactively.

Perfect for Asciinema recordings where you want to show every step you're going
to do on the CLI, without having to have a human do it.

## Example Usage

Given this diff:

```
--- a/ex/galaxy.yml
+++ b/ex/galaxy.yml
@@ -5,9 +5,14 @@
   pre_tasks:
     - name: Install Dependencies
       package:
-        name: 'python3-psycopg2'
+        name: ['acl', 'bzip2', 'git', 'make', 'python3-psycopg2', 'tar', 'virtualenv']
   roles:
     - galaxyproject.postgresql
     - role: natefoo.postgresql_objects
       become: true
       become_user: postgres
+    - geerlingguy.pip
+    - galaxyproject.galaxy
+    - role: uchida.miniconda
+      become: true
+      become_user: "{{ galaxy_user.name }}"
```

(and the `ex/galaxy.yml` of course), it will render this result:

[![asciicast](https://asciinema.org/a/410496.svg)](https://asciinema.org/a/410496)

This asciinema was created by running:

```
asciinema rec -t 'CONDENSED: example' -c 'python diff-player.py --diff ex/ex.diff --speed 0.1 --nosave'
```

Where 0.1 is the time to sleep in seconds between keypresses, and `--nosave` indicates it should not save the file at the end.

## Testing

For testing, take your patch queue, and run them one-by-one without the nosave flag to ensure it matches up with your expected final result.

## Known Issues

- Does not support removed files
- Does not support renamed files

Patches are welcome, I will not be implementing those features myself.

## License

MIT
