iofuncs_sources = files(
    'thread.c',
    'threadset.c',
    'threadpool.c',
    'ginputsource.c',
    'sourceginput.c',
    'connection.c',
    'source.c',
    'sourcecustom.c',
    'target.c',
    'targetcustom.c',
    'sbuf.c',
    'dbuf.c',
    'reorder.c',
    'type.c',
    'gate.c',
    'object.c',
    'error.c',
    'image.c',
    'vips.c',
    'generate.c',
    'mapfile.c',
    'cache.c',
    'sink.c',
    'sinkmemory.c',
    'sinkdisc.c',
    'sinkscreen.c',
    'memory.c',
    'header.c',
    'operation.c',
    'region.c',
    'rect.c',
    'semaphore.c',
    'util.c',
    'init.c',
    'buf.c',
    'window.c',
    'vector.cpp',
    'system.c',
    'buffer.c',
)

iofuncs_headers = files(
    'sink.h',
)

# MSVCでエラーになる (mesonのバグ?) https://github.com/na-trium-144/webcface/actions/runs/12542157738/job/34971586753?pr=441
if glib_dep.type_name() != 'internal'
    vipsmarshal = gnome.genmarshal(
        'vipsmarshal',
        prefix: 'vips',
        sources: 'vipsmarshal.list',
    )
else
    python3 = find_program('python3')
    glib_genmarshal = subproject('glib').get_variable('glib_genmarshal')
    vipsmarshal = [
        custom_target('vipsmarshal-header',
            output: 'vipsmarshal.h',
            input: 'vipsmarshal.list',
            command: [python3, glib_genmarshal, '--quiet', '--prefix', 'vips', '--output', '@OUTPUT@', '--header', '@INPUT@', '--pragma-once' ],
        ),
        custom_target('vipsmarshal-source',
            output: 'vipsmarshal.c',
            input: 'vipsmarshal.list',
            command: [python3, glib_genmarshal, '--quiet', '--prefix', 'vips', '--output', '@OUTPUT@', '--body', '@INPUT@', '--pragma-once' ],
        ),
    ]
endif

libvips_sources += iofuncs_sources

iofuncs_lib = static_library('iofuncs',
    iofuncs_sources,
    iofuncs_headers,
    vipsmarshal,
    dependencies: libvips_deps,
    gnu_symbol_visibility: 'hidden',
)

libvips_components += iofuncs_lib
