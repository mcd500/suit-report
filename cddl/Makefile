# -*- coding: utf-8 -*-
# ----------------------------------------------------------------------------
# Copyright 2022 ARM Limited or its affiliates
#
# SPDX-License-Identifier: Apache-2.0
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.
# ----------------------------------------------------------------------------

CDDL=update-complete.cddl
COSE_XML_URL=https://www.ietf.org/archive/id/draft-ietf-cose-msg-24.xml
COSE_XML=cose.xml
COSE_CDDL=cose.cddl
SUIT_CDDL=suit.cddl
SUIT_CDDL_URL=https://raw.githubusercontent.com/suit-wg/manifest-spec/master/draft-ietf-suit-manifest.cddl
SUIT_MTI_CDDL=suit-mti.cddl
SUIT_MTI_CDDL_URL=https://raw.githubusercontent.com/suit-wg/suit-mti/refs/heads/main/draft-ietf-suit-mti.cddl
SUIT_COMPLETE_CDDL=suit-complete.cddl
UPDATE_CDDL=../draft-ietf-suit-report.cddl

.PHONY: all
all: ${CDDL}

.PHONY: fetch-xml

${COSE_XML}:
	curl -o $@ ${COSE_XML_URL}

${SUIT_CDDL}:
	curl -o $@ ${SUIT_CDDL_URL}

${SUIT_MTI_CDDL}:
	curl -o $@ ${SUIT_MTI_CDDL_URL}

${COSE_CDDL}: ${COSE_XML} cose_cddl.py
	@python3 cose_cddl.py $< | sed -e 's/^!$$//' > $@

${SUIT_COMPLETE_CDDL}: ${SUIT_CDDL} ${SUIT_MTI_CDDL} ${COSE_CDDL}
	cat $^ > $@

${CDDL}: ${UPDATE_CDDL} ${SUIT_COMPLETE_CDDL}
	cat $^ > $@

.PHONY: cddl-syntax-check
cddl-syntax-check: ${CDDL}
	cddlc -2rtcddl $^ > /dev/null || echo CDDL Syntax check ok

.PHONY: clean
clean:
	rm -f ${SUIT_CDDL}
	rm -f ${SUIT_MTI_CDDL}
	rm -f ${SUIT_COMPLETE_CDDL}
