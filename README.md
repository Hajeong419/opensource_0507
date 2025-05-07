def convert_p6_to_p3(input_path, output_path):
    """P6 형식의 PPM 파일을 P3 형식으로 변환합니다."""
    try:
        with open(input_path, 'rb') as infile, open(output_path, 'w') as outfile:
            # 매직 넘버 읽기 (P6)
            magic_number = infile.readline().decode('ascii').strip()
            if magic_number != 'P6':
                raise ValueError("입력 파일이 P6 형식이 아닙니다.")

            # 주석 읽기 (있을 경우)
            line = infile.readline().decode('ascii').strip()
            while line.startswith('#'):
                outfile.write(line + '\n')
                line = infile.readline().decode('ascii').strip()

            # 너비와 높이 읽기
            width, height = map(int, line.split())
            outfile.write(f"P3\n")
            outfile.write(f"# 변환됨\n")
            outfile.write(f"{width} {height}\n")

            # 최대 색상 값 읽기
            max_color_value = int(infile.readline().decode('ascii').strip())
            outfile.write(f"{max_color_value}\n")

            # 픽셀 데이터 읽고 P3 형식으로 쓰기
            for _ in range(height):
                for _ in range(width):
                    r = int.from_bytes(infile.read(1), byteorder='big')
                    g = int.from_bytes(infile.read(1), byteorder='big')
                    b = int.from_bytes(infile.read(1), byteorder='big')
                    outfile.write(f"{r} {g} {b}\n")

        print(f"'{input_path}' 파일이 '{output_path}'로 P3 형식으로 변환되었습니다.")

    except FileNotFoundError:
        print(f"오류: '{input_path}' 파일을 찾을 수 없습니다.")
    except ValueError as e:
        print(f"오류: {e}")
    except Exception as e:
        print(f"알 수 없는 오류 발생: {e}")

if __name__ == "__main__":
    input_file = '/home/data/colorP6File.ppm'
    output_file = 'colorP3File.ppm'
    convert_p6_to_p3(input_file, output_file)
