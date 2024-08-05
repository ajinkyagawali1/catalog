FROM rust:1.73.0-bookworm AS build

WORKDIR /src

COPY ./src /src/src
COPY ./Cargo.toml /src/Cargo.toml
COPY ./spin.toml /src/spin.toml

RUN apt install curl && apt install git
RUN curl -fsSL https://developer.fermyon.com/downloads/install.sh | bash
RUN rustup target add wasm32-wasi
RUN ./spin build


FROM scratch

# Server binary
COPY --from=build /src/ /

# System libraries
COPY --from=build /lib/x86_64-linux-gnu/libc.so.6 				/lib/x86_64-linux-gnu/libc.so.6
COPY --from=build /lib/x86_64-linux-gnu/libm.so.6 				/lib/x86_64-linux-gnu/libm.so.6
COPY --from=build /lib/x86_64-linux-gnu/libgcc_s.so.1 				/lib/x86_64-linux-gnu/libgcc_s.so.1
COPY --from=build /lib64/ld-linux-x86-64.so.2 					/lib64/ld-linux-x86-64.so.2
COPY --from=build /lib/x86_64-linux-gnu/libstdc++.so.6 						/lib/x86_64-linux-gnu/libstdc++.so.6
COPY --from=build /lib/x86_64-linux-gnu/librt.so.1						/lib/x86_64-linux-gnu/librt.so.1
COPY --from=build /lib/x86_64-linux-gnu/libpthread.so.0 						/lib/x86_64-linux-gnu/libpthread.so.0
COPY --from=build /lib/x86_64-linux-gnu/libdl.so.2						/lib/x86_64-linux-gnu/libdl.so.2
COPY --from=build /root/.local/share/spin/plugins/manifests					/root/.local/share/spin/plugins/manifests
COPY --from=build /root/.local/share/spin/plugins/manifests/cloud.json				/root/.local/share/spin/plugins/manifests/cloud.json
COPY --from=build /tmp 										/tmp	
