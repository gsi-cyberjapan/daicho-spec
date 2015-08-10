# [WIP] daicho-spec
Specifications for GSI Tile Daicho

## What is GSI Tile Daicho?
A GSI Tile Daicho is an SQLite3 database to accelerate the production of
[GSI Tile List (mokuroku.csv.gz)](https://github.com/gsi-cyberjapan/mokuroku-spec). Daicho works as a cache of the Mokuroku metadata.

## Database design
### Overview
- location: same as each mokuroku.csv.gz
- file name: daicho.sqlite3
- table name: cache

### Table design
```ruby
$db = Sequel.sqlite('daicho.sqlite3')
unless $db.tables.include?(:cache)
  $db.create_table :cache do
    primary_key String :zxy
    index :zxy, :unique => true
    String :md5
    Integer :mtime
    Integer :size
  end
end
```
The extension of the file must be stored outside of the database to generate path of the tile. The extension of the file is typically stored in [layers.txt](https://github.com/gsi-cyberjapan/layers-dot-txt-spec).

### See Also
See also the database design of client (downloader) side cache md5sum_cache.sqlite3 used in  [qdltc.rb](https://github.com/gsi-cyberjapan/qdltc).

## Initialzing Daicho from Mokuroku
```ruby
init_daicho(mokuroku_in, daicho_path, ext)
```
will be run for each tile dataset.

See [daicho-init](https://github.com/gsi-cyberjapan/daicho-init) for implementation.

## Update Daicho from the filesystem
```ruby
update_daicho(daicho_path, tileset_path, ext)
```
will be run for each tile dataset.

See [daicho-update](https://github.com/gsi-cyberjapan/daicho-update) for implementation.

## Export Mokuroku from Daicho
```ruby
export_mokuroku(daicho_path, mokuroku_out, ext)
```
will be run for each tile dataset.

See [daicho-export](https://github.com/gsi-cyberjapan/daicho-export) for implementation.
