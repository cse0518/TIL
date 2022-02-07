# MultipartFile
> `File`, `FileItem`, `MultipartFile`의 차이를 알고 사용하자.

- 초기에 `File` 타입이 있었다.
- 서블릿 환경에서 Multipart 요청을 위해 아파치에서 `FileItem` 타입을 만들었다.
- 스프링 개발자들이 스프링에서 Multipart 요청으로 쓰기 위해 `MultipartFile` 타입을 만들었다.

<br/>

### File
- 파일 및 디렉터리 경로 이름의 추상적 표현
- 파일 경로를 받아서 파일을 생성한다.
    ```java
    public class File implements Serializable, Comparable<File> {
        
        public File(String pathname) {
            if (pathname == null) {
                throw new NullPointerException();
            }
            this.path = fs.normalize(pathname);
            this.prefixLength = fs.prefixLength(this.path);
        }

        public File(URI uri) {
            if (!uri.isAbsolute())
                throw new IllegalArgumentException("URI is not absolute");
            if (uri.isOpaque())
                throw new IllegalArgumentException("URI is not hierarchical");
            
            String scheme = uri.getScheme();
            if ((scheme == null) || !scheme.equalsIgnoreCase("file"))
                throw new IllegalArgumentException("URI scheme is not \"file\"");
            if (uri.getRawAuthority() != null)
                throw new IllegalArgumentException("URI has an authority component");
            if (uri.getRawFragment() != null)
                throw new IllegalArgumentException("URI has a fragment component");
            if (uri.getRawQuery() != null)
                throw new IllegalArgumentException("URI has a query component");
            
            String p = uri.getPath();
            if (p.isEmpty())
                throw new IllegalArgumentException("URI path component is empty");

            p = fs.fromURIPath(p);
            if (File.separatorChar != '/')
                p = p.replace('/', File.separatorChar);
            
            this.path = fs.normalize(p);
            this.prefixLength = fs.prefixLength(this.path);
        }
    }
    ```

### FileItem
- Multipart/양식 데이터 POST 요청 내에서 수신된 파일 또는 양식 항목을 나타낸다.
- `javax.activation.DataSource`에 의존하지 않고 메소드 구현
  - getInputStream()
  - getContentType()
  - getName()
  ```java
  public interface FileItem extends FileItemHeadersSupport {
  
      // 파일을 메모리에 로드하지 않고 처리할 수 있는 InputStream -> 대용량 파일 처리에 유용
      InputStream getInputStream() throws IOException;
      // 원본 파일 이름 반환
      String getName();
  
      /* FileItem 메소드 */
      // 파일의 모든 내용을 한 번에 요청
      byte[] get() throws UncheckedIOException;
  
      // 지정한 인코딩을 사용하여 파일 항목의 내용을 문자열로 반환
      String getString(String encoding) throws UnsupportedEncodingException, IOException;
      String getString();
  
      void write(File file) throws Exception;
      void delete();
      ... 
  }
  ```

### MultipartFile
- Multipart/양식 데이터 POST 요청으로 수신된 파일의 표현
- 파일 내용은 메모리에 저장되거나 일시적으로 디스크에 저장된다.
  - 임시 저장소는 요청 처리가 끝나면 삭제됨
- `void transferTo(File dest)`
  - 파일 시스템에서 파일을 이동, 복사 저장할 수 있다.
    - 대상 파일이 이미 있으면 먼저 삭제됨
  - 대상 파일이 파일 시스템에서 이동된 경우, 이후에 이 메소드을 다시 호출할 수 없다.(한 번만 호출)
  ```java
  public interface MultipartFile extends InputStreamSource {
  
      String getName();

      @Nullable
      String getOriginalFilename();

      @Nullable
      String getContentType();

      boolean isEmpty();

      long getSize();

      byte[] getBytes() throws IOException;

      @Override
      InputStream getInputStream() throws IOException;
  
      default Resource getResource() {
          return new MultipartFileResource(this);
      }
  
      // 수신된 파일을 지정된 대상 파일로 전송
      void transferTo(File dest) throws IOException, IllegalStateException;
      
      default void transferTo(Path dest) throws IOException, IllegalStateException {
          FileCopyUtils.copy(getInputStream(), Files.newOutputStream(dest));
      }
  }
  ```

<br/>

## Reference
- https://okky.kr/article/1013341
