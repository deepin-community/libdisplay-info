ref_edid_decode = find_program('edid-decode', native: true, required: false)
if not ref_edid_decode.found()
	test('edid-decode-not-found', find_program('false'))
	subdir_done()
endif

test_harness = find_program('./edid-decode-check.sh', native: true)

di_edid_print = executable(
	'di-edid-print',
	'di-edid-print.c',
	dependencies: di_dep,
	install: false,
)

print_harness = find_program('./edid-print-check.sh', native: true)

test_cases = [
	'acer-p1276',
	'apple-xdr-dp',
	'custom-uncommon-cta-vesa',
	'dell-2408wfp-dp',
	'goldstar-ite6604-hdmi',
	'hp-5dq99aa-hdmi',
	'msi-mag321curv-dp',
	'panasonic-mei96a2-dp',
	'samsung-s27a950d-dp',
	'sun-gh19ps-dvi',
	'viewsonic-vp2768-dp',
	'cvt',
	'philips-ftv-490',
	'samsung-q800t-hdmi2.0',
	'cta-timings',
	'hitachi-55r6+',
]

test_env = [
	'REF_EDID_DECODE=' + ref_edid_decode.full_path(),
	'DI_EDID_DECODE=' + di_edid_decode.full_path(),
	'DI_EDID_PRINT=' + di_edid_print.full_path(),
]

foreach tc : test_cases
	test(
		'decode-' + tc,
		test_harness,
		args: [files('data/' + tc + '.edid')],
		env: test_env,
		depends: [di_edid_decode],
	)
	test(
		'print-' + tc,
		print_harness,
		args: [files('data/' + tc + '.edid')],
		env: test_env,
		depends: [di_edid_print],
	)
endforeach

test_gen = find_program('./edid-decode-diff.sh', native: true)

gen_targets = []
foreach tc : test_cases
	gen_targets += files('data/' + tc + '.edid')
endforeach

run_target(
	'gen-test-data',
	command: [test_gen] + gen_targets,
	depends: [di_edid_decode, di_edid_print],
	env: test_env,
)
