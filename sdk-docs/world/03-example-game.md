---
title: 예제 게임
---

export const GitHubRedirect = () => {
  return (
    <div style={{ textAlign: 'center', padding: '40px' }}>
      <p>예제 게임 코드를 GitHub에서 확인하세요!</p>
      <a 
        href="https://github.com/afi-backnd" 
        target="_blank" 
        rel="noopener noreferrer"
        style={{
          display: 'inline-block',
          padding: '12px 24px',
          backgroundColor: '#0969da',
          color: 'white',
          textDecoration: 'none',
          borderRadius: '6px',
          fontWeight: 'bold'
        }}
      >
        🔗 GitHub에서 보기
      </a>
    </div>
  );
};

<GitHubRedirect />
